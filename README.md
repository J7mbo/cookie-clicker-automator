# cookie-clicker-automator
I have no idea why I've just done this. A friend said "wouldn't it be awesome if we could automate the new cookie clicker", cue me wasting an hour on a Saturday evening building this that does the following:

- Automatically click the cookie every 1 millisecond
- Automatically purchase the lowest costing purchase every 2 seconds

Paste the following in your JavaScript console:

  function doPurchaseClick() 
  {
  	var numCookies = parseFloat(document.querySelector('#cookies.title').innerHTML.match(/[0-9]+,[0-9]+|[0-9]+ cookies|[0-9]+.[0-9]+ million/g)[0].replace('cookies', '').replace('million', ''))
  	var isMillion = (document.querySelector('#cookies.title').innerHTML.match(/[0-9]+ million/g) != false);
  
  	/** 1.4 million needs to be come 1400000 (remove full stop, add 5 "0"s then parse to float **/
  	if (isMillion != false) {
  		numCookies = parseInt(numCookies + "00000");
  	}
  	
  	/** All the enabled purchases **/
  	var enabledPurchases = document.querySelectorAll('.product.unlocked.enabled');
  	
  	/** Store all the purchase costs in an array **/
  	var costs = [];
  	
  	for (var i = 0; i < enabledPurchases.length; i++) {
  		var cost = parseFloat(document.querySelectorAll('.product.unlocked.enabled > div.content > span.price')[i].innerHTML.replace(/,/, ''))
  		costs.push(cost);
  	}
  	
  	/** Get the lowest cost purchase key so we know which of the enabled purchases is the one to buy **/
  	var lowestCost = Math.min.apply(Math, costs);
  	
  	var lowestPurchaseKey = 0;
  	
  	for (var x = 0; x < costs.length; x++) {
  		if (costs[x] == lowestCost) {
  			lowestPurchaseKey = x;
  		}
  	}
  	
  	var purchaseToClick = enabledPurchases[lowestPurchaseKey];
  	
  	if (purchaseToClick) {
  		purchaseToClick.click();
  	}
  }
  
  function doCookieClick() {
  	document.querySelector('div#bigCookie').click();
  }
  
Then, to run the code, paste these lines in your console:
  
  /** To run the loops **/
  var cookieClickLoop = setInterval(doCookieClick, 1);
  var purchaseClickLoop = setInterval(doPurchaseClick, 1000);
  
To stop the loops, paste these lines in your console:

  /** To stop the loops **/
  clearInterval(cookieClickLoop);
  clearInterval(purchaseClickLoop);
