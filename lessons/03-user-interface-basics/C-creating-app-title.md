Create a new View called AppTitle, setting up the Logo as an Image asset (you can use PNG, SVG or PDF versions from the assets folder). Call the image in the Assets "LogoCM"

```swift
struct AppTitle: View {
    var body: some View {
        HStack {
            Spacer()
            Image("LogoCM")
            Spacer()
        }
            .padding(4)
            .listRowBackground(Color("Secondary"))
            .background(Color("Secondary"))
    }
}
```

