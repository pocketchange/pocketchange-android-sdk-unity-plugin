# Pocket Change's Android SDK Plugin for Unity (Products).

## Step 1: Complete the basic Android SDK Plugin integration

Follow all of the steps in the <a href="https://github.com/pocketchange/pocketchange-android-sdk-unity-plugin/blob/master/README.md">README</a>. Once your application functions correctly with the the basic plugin components, you can add products to it.

## Step 2: Integrate products into your app

**REMINDER**: You must load a scene that includes either the PocketChangeAndroidPlayerInitializer or PocketChangeAndroidSceneInitializer prefab before calling any plugin methods.

To initiate an item purchase, call the `PocketChangeAndroid.PurchaseItem` method with the item's SKU as an argument:

```C#
PocketChangeAndroid.PurchaseItem("an-item-sku");
```

When the user triggers a UI event to consume an item, call:

```C#
if (PocketChangeAndroid.UseItem("an-item-sku", amountToUse)) {
    // use the item
}
```

The `UseItem` method accepts two arguments:

1. sku (String): The SKU of the item to use
2. amount (int): The amount to use

The `UseItem` method returns true if the user's account was successfully debited, in which case you should credit the user for consuming the given amount of the item; if the method returns false, update the UI as appropriate and do not credit the user.

To determine the user's current inventory for an item, call:

```C#
PocketChangeAndroid.GetItemCount("an-item-sku")
```

The `GetItemCount` method returns an int representing the current balance for the item with the provided SKU. This method allows your game to:

* Check for the presence of non-consumable items.
* Show a complete inventory to the user by iterating over all of the SKUs in a given release.

