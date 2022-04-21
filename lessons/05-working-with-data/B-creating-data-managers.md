In the Model folder we will create some manager objects that will be used as singletons in our app

## MenuManager

```swift
class MenuManager {    
    var menu: [Category] = [] 
}
```

## CartManager

```swift
class CartManager {
    var products: [(Product, Int)] = []
        
    func add(product: Product, quantity: Int) {
        products.append((product, quantity))
    }
        
    func remove(product: Product) {
        products.removeAll { productInCart in
            productInCart.0.id == product.id
        }
    }
    
    func clear() {
        products.removeAll()
    }
    
    func total() -> Double {
        var total = 0.0
        for item in products {
            total += item.0.price * Double(item.1)
        }
        return total
    }
}
```

