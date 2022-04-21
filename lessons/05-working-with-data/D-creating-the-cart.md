Let's work with the Cart now.

Now that we have a Cart Manager and we can inject it, we can make our "add to cart" button work within details page.

## DetailsPage

```swift
//
//  DetailsPage.swift
//  CoffeeMastersTest1
//
//  Created by Maximiliano Firtman on 4/14/22.
//

import SwiftUI

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
    }
}

struct DetailsPage_Previews: PreviewProvider {
    static var previews: some View {
        NavigationView {
            DetailsPage(product: Product(id: 0, name: "Demo Product", description: "Demo description", price: 23, image: "blacktea.png"))
                .environmentObject(LikesManager())
            .previewDevice("iPhone 11 Pro")
        }
    }
}

struct ProductThumbnail: View {
    var url: URL
    
    var body: some View {
        AsyncImage(url: url)
            .cornerRadius(5)
            .frame(maxWidth: .infinity, idealHeight: 150, maxHeight: 150)

    }
}
```

## OrderPage

```swift
struct OrderPage: View {
    
    @EnvironmentObject var menuManager: MenuManager
    @EnvironmentObject var cartManager: CartManager
    
    @AppStorage("name") var name = ""
    @AppStorage("phone") var phone = ""
    
    @State var orderConfirmed = false

    var body: some View {
        
        NavigationView {
            if cartManager.products.count == 0 {
                Text("Your order is empty")
                    .navigationTitle("Your Order")
            } else {
                List {
                    Section("ITEMS") {
                        ForEach(cartManager.products, id:\.0.id) { item in
                            OrderItem(item: item)
                        }
                    }.listRowBackground(Color("Background"))
                                        
                    Section("YOUR DETAILS") {
                        VStack {
                            TextField("Your Name", text: $name)
                                .textFieldStyle(.roundedBorder)
                            Spacer().frame(height: 20)
                            TextField("Your Phone #", text: $phone)
                                .keyboardType(.phonePad)
                                .textFieldStyle(.roundedBorder)
                        }.padding(.top)
                         .padding(.bottom)
                    }.listRowBackground(Color("Background"))
                    
                    Section() {
                        HStack {
                            Spacer()
                            Text("Total")
                            Spacer()
                            Text("$ \(cartManager.total(), specifier: "%.2f")")
                                .bold()
                            Spacer()
                        }
                    }.listRowBackground(Color.clear)
                    
                    Section {
                        HStack {
                            Spacer()
                            Button("Place Order") {
                                //TODO: Validation
                                orderConfirmed = true
                            }
                                .padding()
                                .frame(width: 250.0)
                                .background(Color("Alternative2"))
                                .foregroundColor(Color.black)
                                .cornerRadius(25)
                                
                            Spacer()
                        }
                    }.listRowBackground(Color.clear)
                }
                .listSectionSeparatorTint(Color("AccentColor"))
                .listStyle(.insetGrouped)
                .navigationTitle("Your Order")
                .alert("Order", isPresented: $orderConfirmed, actions: {
                    Button("OK", role: .cancel) {
                        //TODO: send order
                        orderConfirmed = false
                        cartManager.clear()
                    }
                }, message: {
                    Text("Your order is being prepared.")
                        .font(.title)
                })
            }
        }
    }
}
```

