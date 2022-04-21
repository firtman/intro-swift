Create the Offer view in a new file, and use the following code as a base:

```swift
struct Offer: View {
    var title: String = ""
    var description: String = ""
    
    var body: some View {
        ZStack {
            Image("BackgroundPattern")
                .frame(maxWidth: .infinity, maxHeight: 300)
                .clipped()
            VStack(alignment: .center, spacing: 50) {
                VStack {
                    Text(title)
                        .padding()
                        .font(.title)
                        .background(Color("CardBackground"))
                        .padding(.bottom, 20)
                    Text(description)
                        .padding()
                        .background(Color("CardBackground"))
                }
                .frame(maxWidth: .infinity, minHeight: 150, maxHeight: 150, alignment: .center)
            }
        }.background(Color.white)
        
    }
}
```

Now create the OffersView whose body should look like:

```swift
NavigationView {
    VStack {
        List {
            Offer(title: "Early Coffee", description: "10% off. Offer valid from 6am to 9am.")
            Offer(title: "Welcome Gift", description: "25% off on your first order.")
        }.listStyle(.plain)
            .listRowSeparator(.hidden)
    }.navigationTitle("Offers")
}
```
