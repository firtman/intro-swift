We are going to setup some basic tabs, so open ContentView and change the current body with:

```swift
TabView {
    Text("Home")
        .tabItem {
            Image(systemName: "cup.and.saucer")
            Text("Menu")
        }
    Text("Offers")
        .tabItem {
            Image(systemName: "tag")
            Text("Offers")
        }
    Text("My order")
        .tabItem {
            Image(systemName: "cart")
            Text("My Order")
        }
    Text("Info")
        .tabItem {
            Image(systemName: "info.circle")
            Text("Info")
        }
}
```

## Icons

Icons used in the previous sample as coming from *SFSymbols*, a set of free to use icons from Apple. Download the [SF Symbols app](https://developer.apple.com/sf-symbols/) to browse all the options available.

## Creating Pages

Now create a Pages group and create one view per page, such as
* MenuPage
* OffersPage
* OrderPage
* InfoPage

Replace the `Text` views in the `TabView` with these new views. Your ContentView should look now like:

```swift
TabView {
    MenuPage()
        .tabItem {
            Image(systemName: "cup.and.saucer")
            Text("Home")
        }
    OffersPage()
        .tabItem {
            Image(systemName: "tag")
            Text("Offers")
        }
    OrderPage()
        .tabItem {
            Image(systemName: "cart")
            Text("My Order")
        }
    InfoPage()
        .tabItem {
            Image(systemName: "info.circle")
            Text("Info")
        }
}
```