<!DOCTYPE html>
<html>
<head>
    <title>Online Shop</title>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
</head>
<body>
    <h1>Online Shop</h1>
    <div id="products">
        <!-- Lägg till fler produkter här -->

        <div class="product">
            <h2>Product 1</h2>
            <p>Price: 100 SEK</p>
            <button onclick="addItemToList(1), UpdateCartTotal(1)">Add to Cart</button>
        </div>

        <div class="product">
            <h2>Product 2</h2>
            <p>Price: 200 SEK</p>
            <button onclick="addItemToList(2), UpdateCartTotal(2)">Add to Cart</button>
        </div>

    </div>
    <!-- räknare för produkter, försök flytta till ett lämpligt ställe-->
    <div>
        <button onclick="decrement()">-</button>
        <span id="counter">0</span>
        <button onclick="increment()">+</button>
    </div>

    <!-- Kundvagnen, lägg till funktion för att visa/dölja, använd firebase för att spara/hämta kundinformation -->

    <div id="cart" class="hidden">
        <h2>Shopping Cart</h2>
        <ul id="CartItems"></ul>
        <ul>Total: <span id="CartTotal"></span> SEK = 
            <span id="BitcoinCartTotal"></span> BTC = 
            <span id="LitecoinCartTotal"></span> LTC = 
            <span id="EthereumCartTotal"></span> ETH = 
            <span id="DogecoinCartTotal"></span> DOGE</ul>
        <a href="checkout.html">
            <button>Proceed to checkout</button>
            </a>
        <p>1 BTC = <span id="bitcoin"></span> SEK</p>
        <p>1 LTC = <span id="litecoin"></span> SEK</p>
        <p>1 ETH = <span id="ethereum"></span> SEK</p>
        <p>1 DOGE = <span id="dogecoin"></span> SEK</p>
    </div>

    <script>
        //Javascript för funktioner, om du lägger till fler produkter kom ihåg att lägga in dom här också :) Sexigt med hårdkodning
        //lägg till räknare för fler krypto valutor
        let Total = 0;
        let BitcoinTotal = 0;
        let LitecoinTotal = 0;
        let EthereumTotal = 0;
        let DogecoinTotal = 0;

        updateText();
        function UpdateCartTotal(n) {
            if (n==1)
                Total += 100;
                updateText();
            if (n==2)
                Total += 200;
                updateText();

            const bitcoinValue = parseFloat(document.getElementById('bitcoin').textContent);
            const litecoinValue = parseFloat(document.getElementById('litecoin').textContent);
            const ethereumValue = parseFloat(document.getElementById('ethereum').textContent);
            const dogecoinValue = parseFloat(document.getElementById('dogecoin').textContent);

            BitcoinTotal = (1 / bitcoinValue) * Total;
            LitecoinTotal = (1 / litecoinValue) * Total;
            EthereumTotal = (1 / ethereumValue) * Total;
            DogecoinTotal = (1 / dogecoinValue) * Total;
            updateText();
        }

        function updateText() {
            document.getElementById('CartTotal').innerText = Total;
            document.getElementById('BitcoinCartTotal').innerText = BitcoinTotal;
            document.getElementById('LitecoinCartTotal').innerText = LitecoinTotal;
            document.getElementById('EthereumCartTotal').innerText = EthereumTotal;
            document.getElementById('DogecoinCartTotal').innerText = DogecoinTotal;
        }
        //lägg till produkter här också
        function addItemToList(n) {
            const itemList = document.getElementById("CartItems");
            const newItem = document.createElement("li");
            if (n==1)
                newItem.textContent = "Product 1";
            if (n==2)
                newItem.textContent = "Product 2";

            itemList.appendChild(newItem);
        }

        //Accepterade krypto valutor samt api för att hämta live priser för dom
        var btc = document.getElementById("bitcoin");
        var ltc = document.getElementById("litecoin");
        var eth = document.getElementById("ethereum");
        var doge = document.getElementById("dogecoin");

        var liveprice = {
            "async": true,
            "scroosDomain": true,
            "url": "https://api.coingecko.com/api/v3/simple/price?ids=bitcoin%2Clitecoin%2Cethereum%2Cdogecoin&vs_currencies=sek",
            "method": "GET",
            "headers": {}
        }

        $.ajax(liveprice).done(function (response){
            btc.innerHTML = response.bitcoin.sek;
            ltc.innerHTML = response.litecoin.sek;
            eth.innerHTML = response.ethereum.sek;
            doge.innerHTML = response.dogecoin.sek;

        });

        let counter = 0;
        const counterElement = document.getElementById("counter");

        function increment() {
            counter++;
            counterElement.textContent = counter;
        }

        function decrement() {
            if (counter > 0) {
                counter--;
                counterElement.textContent = counter;
            }
        }
    </script>
</body>
</html>
