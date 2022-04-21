A bonus to this project is to add favorites or liked products and save it locally.

## LikesManager
```swift
class LikesManager: ObservableObject {
    static let key = "likes"
    @Published var likes: [Int] = []
    
    init() {
        likes = UserDefaults.standard.array(forKey: LikesManager.key) as? [Int] ?? []
    }
    
    func isLiked(id: Int) -> Bool {
        return likes.contains(id)
    }
    
    func toggle(_ id: Int) {
        if isLiked(id: id) {
            likes.removeAll { $0 == id }
        } else {
            likes.append(id)
        }
        UserDefaults.standard.set(likes, forKey: LikesManager.key)
    }
}
```

## LikeButton View
```swift
struct LikeButton: View {
    @EnvironmentObject var likesManager: LikesManager
    
    var product: Product
    
    var body: some View {
        Image(systemName: likesManager.isLiked(id: product.id) ? "heart.fill" : "heart")
            .padding()
            .foregroundColor(Color("Secondary"))
            .accessibilityLabel(likesManager.isLiked(id: product.id) ? "Dislike" : "Like")
            .onTapGesture {
                likesManager.toggle(product.id)
            }
    }
}
```

## Usage on ProductItem
```swift
struct ProductItem: View {
    var product: Product
    
    var body: some View {
        VStack{
            AsyncImage(url: product.imageURL)
                .frame(width: 300, height: 150)
                .background(Color("AccentColor"))
            HStack {
                VStack(alignment: .leading) {
                    Text(product.name)
                        .font(.title3)
                        .bold()
                    Text("$ \(product.price, specifier: "%.2f")")
                        .font(.caption)

                }.padding(8)
                Spacer()
                LikeButton(product: product)
            }
        }
        .background(Color("SurfaceBackground"))
        .cornerRadius(10)
        .padding(.trailing)

    }
}
```

## Usage on DetailsPage

```swift
struct DetailsPage: View {
    
    var product: Product
    @State var quantity: Int = 1
    @EnvironmentObject var cartManager: CartManager
    @Environment(\.dismiss) var dismiss
        
    var body: some View {
        ScrollView {
            ProductThumbnail(url: product.imageURL)
                .padding(.top, 32)
            Text(product.description ?? "")
                .frame(maxWidth: .infinity)
                .multilineTextAlignment(.leading)
                .padding(24)
            HStack {
                Text("$ \(product.price, specifier: "%.2f") ea")
                Stepper(value: $quantity, in: 1...10) { }
            }
                .frame(maxWidth: .infinity)
                .padding(30)
            
            let total = product.price * Double($quantity.wrappedValue)
                        
            Text("Subtotal $\(total, specifier: "%.2f")")
                .bold()
                .padding(12)
            
            Button("Add \(quantity) to Cart") {
                dismiss()
                cartManager.add(product: product, quantity: quantity)
            }
                .padding()
                .frame(width: 250.0)
                .background(Color("Alternative2"))
                .foregroundColor(Color.black)
                .cornerRadius(25)

        }
            .navigationTitle(product.name)
            .toolbar {
                LikeButton(product: product)
            }
    }
}
```