# AEPriceMatrix

Tier based currency conversion for iOS based on the [App Store Pricing Matrix][priceMatrix].

## Overview

Example usage:
```objc
NSNumber *converted = [AEPriceMatrix convert:@0.99 from:@"USD" to:@"EUR"];
NSLog(@"converted to %@", converted);
```
Output:
```
converted to 0.89
```

## In-App Purchase Tracking

This can be used for In-App Purchase tracking in a specific currency regardless
of the local currency that the purchase was paid in. The following example
tracks all revenue in `EUR`.  See [the In-App Purchase Programming
Guide][inAppGuide] for details on how to implement a method like
`productForId:` that finds an `SKProduct` by its product identifier.

```objc
- (void)paymentQueue:(SKPaymentQueue *)queue updatedTransactions:(NSArray *)transactions {
    for (SKPaymentTransaction *transaction in transactions) {
        switch (transaction.transactionState) {
            case SKPaymentTransactionStatePurchased:
                [self finishTransaction:transaction];

                NSString *productId = transaction.payment.productIdentifier;
                SKProduct *product = [self productForId:productId];
                NSString *currency = [product.priceLocale objectForKey:NSLocaleCurrencyCode];
                NSNumber *price = product.price;

                NSNumber *revenue = [AEPriceMatrix convert:price from:currency to:@"EUR"];
                [self trackRevenue:revenue];

                break;
            // more cases
        }
    }
}
```

## Available Currency Codes

Instead of plain currency code strings like `@"USD"` you can also use the
following string constants:

```objc
kAECurrencyUSD    // U.S.
kAECurrencyCAD    // Canada
kAECurrencyMXN    // Mexico
kAECurrencyAUD    // Australia
kAECurrencyNZD    // New Zealand
kAECurrencyJPY    // Japan
kAECurrencyEUR    // Europe
kAECurrencyCHF    // Switzerland
kAECurrencyNOK    // Norway
kAECurrencyGBP    // U.K
kAECurrencyDKK    // Denmark
kAECurrencySEK    // Sweden
kAECurrencyCNY    // China
kAECurrencySGD    // Singapore
kAECurrencyHKD    // Hong Kong
kAECurrencyTWD    // Taiwan
kAECurrencyRUB    // Russia
kAECurrencyTRY    // Turkey
kAECurrencyINR    // India
kAECurrencyIDR    // Indonesia
kAECurrencyILS    // Isreal
kAECurrencyZAR    // South Africa
kAECurrencySAR    // Saudi Arabia
kAECurrencyAED    // UAE
```

[priceMatrix]: http://forecomm.mptw.fr/templates/PriceMatrix.html
[inAppGuide]: https://developer.apple.com/Library/ios/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Chapters/ShowUI.html
