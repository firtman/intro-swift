We will introduce in the View structure objects that we can use in any place to avoid passing data manually as properties. These are called `Environment Objects` and we can inject them at any level, but we will do it at the app level.

## App level

```swift 
@StateObject var menuManager = MenuManager()
@StateObject var cartManager = CartManager()
    
var body: some Scene {
    WindowGroup {
        ContentView()
            .environmentObject(cartManager)
            .environmentObject(menuManager)
    }
}
```    

To make it work, MenuManager and CartManager have to conform to `ObservableObject` and we should use `@Published` at the properties we want to bind data to:

## CartManager

```swift
class CartManager: ObservableObject {
    @Published var products: [(Product, Int)] = []
    // ...
}
```

## MenuManager
```swift
class MenuManager: ObservableObject {  
    @Published var menu: [Category] = []
}
```

To use these objects we must declare `@EnvironmentObject` variables in our views without instanciating these objects (they will be injected), such as:

```swift
@EnvironmentObject var menuManager: MenuManager
```

The MenuPage now can be upgraded into:

```swift
struct MenuPage: View {
    
    @EnvironmentObject var menuManager: MenuManager
    
    var body: some View {
        VStack {
            NavigationView {
                    List {
                        AppTitle()
                            .padding(.top, 4)
                        if menuManager.menu.count == 0 {
                            HStack {
                                Text("We couldn't fetch the data")
                                Button("Reload") {
                                }
                            }
                        } else {
                            ForEach(menuManager.menu) { category in
                                if category.products.count > 0 {
                                    Text(category.name)
                                    .listRowBackground(Color("Background"))
                                    .foregroundColor(Color("Secondary"))
                                    .padding()
                                }
                                
                                ForEach(category.products) { item in
                                    ZStack {
                                        NavigationLink(destination: DetailsPage(product: item)) {
                                            EmptyView()
                                        }.opacity(0)
                                        ProductItem(product: item)
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
    }
}
```

## Identifiable Types

We will have to conform to Identifiable on Product and Category structs

```swift
struct Product: Identifiable {
    var id: Int
    var name: String
    var description: String?
    var price: Double
    var image: String = ""
    
    var imageURL: URL {
        URL(string: "https://firtman.github.io/coffeemasters/api/images/\(self.image)")!
    }
}

struct Category: Identifiable {
    var id: String { name }
    var name: String
    var products: [Product] = []
}
```