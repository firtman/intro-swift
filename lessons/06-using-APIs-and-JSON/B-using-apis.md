We will now use Alamofire to download a JSON from an API. We will be doing it within ProductManager

```swift
class MenuManager: ObservableObject {
    
    @Published var menu: [Category] = []
    
    init() {
        refreshItemsFromNetwork()
    }
    
    func refreshItemsFromNetwork()  {
        AF.request("https://firtman.github.io/coffeemasters/api/menu.json")
            .responseDecodable(of: [Category].self) { response in
                if let menuFromNetwork = response.value {
                    self.menu = menuFromNetwork
                }
            }
    }
}
```

## Decodable Types

To parse automatically the JSON into our structures, they must conform to `Decodable`


```swift
struct Product: Identifiable, Decodable {
    var id: Int
    var name: String
    var description: String?
    var price: Double
    var image: String = ""
    
    var imageURL: URL {
        URL(string: "https://firtman.github.io/coffeemasters/api/images/\(self.image)")!
    }
}

struct Category: Identifiable, Decodable {
    var id: String { name }
    var name: String
    var products: [Product] = []
}
```

## MenuPage

We can now make our List refreshable by adding to the List a modifier

```swift
.refreshable {
    menuManager.refreshItemsFromNetwork()
}
```
