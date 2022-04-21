It's time to start working with data. Let's create a Model group where we will create the following files:

## Product.swift

```swift
struct Product {
    var id: Int
    var name: String
    var description: String?
    var price: Double
    var image: String = ""
    
    var imageURL: URL {
        URL(string: "https://firtman.github.io/coffeemasters/api/images/\(self.image)")!
    }
}
```

## Category.swift

```swift
struct Category {
    var name: String
    var items: [Product] = []    
}
```

## ProductItem

The product now has imageURL, so we can replace `Image` with `AsyncImage`, and receive a product in ProductItem such as:

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
            }
        }
        .background(Color("SurfaceBackground"))
        .cornerRadius(10)
        .padding(.trailing)

    }
}
```

Adjust all usages to this new pattern.

