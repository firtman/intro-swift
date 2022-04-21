Create a DetailsPage that will be used for receiving links from the Menu section.


Your DetailsPage can look like:

```swift
ScrollView {
    Image("DummyImage")
        .cornerRadius(5)
        .frame(maxWidth: .infinity, idealHeight: 150, maxHeight: 150)
        .padding(.top, 32)
    Text("Product")
        .frame(maxWidth: .infinity)
        .multilineTextAlignment(.leading)
        .padding(24)
    HStack {
        Text("$ 4.25 ea")
        Stepper(value: $quantity, in: 1...10) { }
    }
        .frame(maxWidth: .infinity)
        .padding(30)
                    
    Text("Subtotal $4.25")
        .bold()
        .padding(12)
    
    Button("Add \(quantity) to Cart") {
        //TODO
    }
        .padding()
        .frame(width: 250.0)
        .background(Color("Alternative2"))
        .foregroundColor(Color.black)
        .cornerRadius(25)

}
    .navigationTitle(product.name)
```

We need to add a `@State` quantity variable to the struct to make this work.

Next, to make the button go back to previous page, let's introduce an Environment variable:

```swift
    @Environment(\.dismiss) var dismiss
```

Now it's time to go back to the Product List and add a link to details, wrapping the ProductItem with a NavigationLink

```swift
NavigationLink(destination: DetailsPage()) {
   ProductItem()
        .padding(.top)
        .padding(.leading)
        .padding(.bottom, 12) 
}
```

To remove the automatic arrow in the list, we have to do some tricks

```swift
ZStack {
    NavigationLink(destination: DetailsPage()) {
        EmptyView()
    }.opacity(0)
    ProductItem()
        .padding(.top)
        .padding(.leading)
        .padding(.bottom, 12)

}
```