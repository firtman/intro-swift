Create a ProductItem view with something similar to:

```swift
struct ProductItem: View {
            
    var body: some View {
        VStack{
            Image("DummyImage")
                .frame(width: 300, height: 150)
                .background(Color("AccentColor"))
            HStack {
                VStack(alignment: .leading) {
                    Text("Product Name")
                        .font(.title3)
                        .bold()
                    Text("$ 4.25")
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

Now, in MenuPage, let's use the new ProductItem

```swift
VStack {
    NavigationView {
        List {
            AppTitle()
                .padding(.top, 4)
            ForEach(1 .. <3) { category in
                Text("HOT COFFEE")
                    .listRowBackground(Color("Background"))
                    .foregroundColor(Color("Secondary"))
                    .padding()
            
                ForEach(1 .. < 5>) { item in                               
                    ProductItem()
                        .padding(.top)
                        .padding(.leading)
                        .padding(.bottom, 12)
                    }
                    
                }
            }
                .listRowInsets(EdgeInsets(top: 0, leading: 0, bottom: 0, trailing: 0))
                .listRowSeparator(.hidden)
                .listRowBackground(Color("Background"))
            }
        }
        .listStyle(.insetGrouped)
        .navigationTitle("Products")
        .background(Color("SurfaceBackground"))            
    }


}
.navigationViewStyle(StackNavigationViewStyle())
```