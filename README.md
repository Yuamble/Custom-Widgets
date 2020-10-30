# Custom Widgets
## Widget Ethereum price on iOS 14
- For using code, download app Scriptable(free)
- Create workspace in app (tap +)
- Copy and paste code
- Then, add medium size widget on homesreen of Scriptable
- Long tap on widget and tap "change "Scriptable""
- Choose your file and that's all! Widget is ready!
**(Don't forget to change API key in code!!!)**
Code:
```javascript
let apikey = "YOUR API KEY"
let ether_image_url = "https://s2.coinmarketcap.com/static/img/coins/64x64/1027.png"
let price_ether_url = "https://api.etherscan.io/api?module=stats&action=ethprice&apikey=" + apikey
let gas_price_url = "https://api.etherscan.io/api?module=gastracker&action=gasoracle&apikey=" + apikey


let price_text = new Request(price_ether_url)
price_text = await price_text.loadJSON()
price_text = price_text["result"]["ethusd"]
let icon_logo = await Loadimage(ether_image_url)

let gas_text = new Request(gas_price_url)
gas_text = await gas_text.loadJSON()
gas_text = gas_text["result"]["ProposeGasPrice"]

let widget = createWidget()
// widget.backgroundImage = await Loadimage(background_url)
let gradient = new LinearGradient();
gradient.colors = [new Color("#1a1a1a"), new Color("#142161")];
gradient.locations = [0, 1.5];
widget.backgroundGradient = gradient;

if (config.runsInWidget) {
    Script.setWidget(widget)
    Script.complete()
} else {
    widget.presentMedium()
}

function createWidget() {
    let widget = new ListWidget()
    
    let stack1 = widget.addStack()
    stack1.centerAlignContent()
    let icon = stack1.addImage(icon_logo)
    icon.imageSize = new Size(30, 30)

    let spacer1 = stack1.addSpacer(10);
    
    let text_ether = stack1.addText("Ethereum")
    text_ether.font = Font.boldRoundedSystemFont(20)
    text_ether.textColor = new Color("#f0f0f0")
    
    let spacer11 = stack1.addSpacer(10);
    
    let text_ETH = stack1.addText("(ETH)")
    text_ETH.font = Font.mediumRoundedSystemFont(15)
    text_ETH.textColor = new Color("#a7acc4")

    let spacer2 = widget.addSpacer(10)
    let price = widget.addText(String(price_text + " $"))
    price.font = Font.mediumRoundedSystemFont(30)
    price.textColor = new Color("#f0f0f0")
    
    let spacer3 = widget.addSpacer(5)
    
let gasprice = widget.addText("⛽ " + gas_text + " Gwei (" + (parseInt(gas_text)/1000000000*21000*parseFloat(price_text)).toFixed(2)+ "$)")
    gasprice.font = Font.mediumRoundedSystemFont(18)
    gasprice.textColor = new Color("#a7acc4")

    let spacerdown = widget.addSpacer();
    
    return widget
}

async function Loadimage(url) {
    let req = new Request(url)
    return await req.loadImage()
}
```
