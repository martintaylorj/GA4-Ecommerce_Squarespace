# GA4-Ecommerce_Squarespace
This is a Google Tag Manager implementation guide to help Squarespace users track their ecommerce data in Google Analytics 4 (GA4).

Currently, Squarespace's native GA4 integration only tracks purchases. But if ecommerce analytics is crucial for your business, I suggest utilizing Google Tag Manager to track the entire purchase flow instead of relying on the native GA4 integration. This way, you'll have the ability to:

- See which products customers add and remove the most from their cart

- Analyze what pages people visit on your website before and after entering the purchase flow

- View how many people leave your checkout flow before purchasing

- Discover which products users view the most

**Prerequisites** Before getting started, you’ll need to create a GA4 ecommerce tag in Google Tag Manager. This is the tag that will actually send all of your ecommerce data to GA4, and fortunately it's pretty simple to set up!

### How to Set Up the GA4 Event Tag Configuration in Google Tag Manager
<details>
<summary>Expand to see a screenshot of GA4 Event Tag Configuration</summary>
![image](https://github.com/martintaylorj/GA4-Ecommerce_Squarespace/assets/81248339/5e5d76ff-b6d3-48bd-ba65-92cc4f9a24af)
</details>
**Tag Type:** Google Analytics: GA4 Event
**Event Name**: {{Event}} - this will push the same event name that's in the data layer to your GA4 property
**Ecommerce**: Check the "Send Ecommerce data" option
**Data source**: Data Layer

#### Trigger Configuration
<details>
<summary>Expand to see a screenshot of GA4 Event Trigger Configuration</summary>
![ga4-ecommerce-ga4 trigger setup in gtm](https://github.com/martintaylorj/GA4-Ecommerce_Squarespace/assets/81248339/75b7b1a8-653f-4cd5-892e-1fa8f6eb3148)
</details>

**Trigger Type**: Custom Event (these are the event names for each ecommerce event. I've also included the promotion events which I won't cover in this guide)
Event name: view_promotion|select_promotion|view_item_list|select_item|view_item|add_to_cart|view_cart|begin_checkout|remove_from_cart|purchase
  Check "Use regex matching"

# How to Set Up GA4 Ecommerce Events Using Google Tag Manager
In this section, I’ll guide you through setting up new tags, triggers, and variables to send your ecommerce events to GA4 for the followng events. 
If you're only interested in a particular event, click the link to skip to that section.

- **view_item_list**: When a user views a list of products or offerings. Scroll to [this section](#view-item-list)

- **view_item**: When a user views a product. Scroll to [this section](#view_item)

- **select_item**: When a user selects a product from a list of items or offerings                                                                      

- **add_to_cart**: When a user adds products to their shopping cart

- **view_cart**: When a user views their shopping cart

- **remove_from_cart**: When a user removes items from their shopping cart (my solution will work when the user clicks on the decrement button to remove one product and when the user clicks on the “x” button to completely remove all products)

- **begin_checkout**: When a user begins checkout (clicks on checkout button)

- **purchase**: When a user makes a purchase

All of these ecommerce events will be implemented using GTM’s custom HTML tag type:

## view item list
[Get the custom code here in my repo.](ga4-ecommerce-view_item-list.html)

### Trigger: 
**Trigger Type**: Page View - Window Loaded
**This trigger fires on**: Page Path equals /store

💡I chose the Window Loaded trigger type to ensure the HTML elements were fully loaded on the page before firing the tag. This is is especially critical for tags where we attach event listeners to elements that might be rendered after the initial page load.

## view_item: When a user views a product.
[Get the custom code here in my repo.](ga4-ecommerce-view_item.html)

### Trigger: 
There are 2 triggers for this tag
1. **Trigger Type**: Custom Event name: ssRawProductDetailPush OR Element Visibility > Selection Method > CSS Selector > Element Selector: .sqs-product-quick-view-content
   OR
2.**Trigger Type**:  Element Visibility > Selection Method > CSS Selector > Element Selector: .sqs-product-quick-view-content

💡For my Squarespace template, there are two ways for a user to view an item, by clicking the quick view button and seeing the quick view popup or by viewing the full product page. When the full product page is viewed, Squarespace automatically pushes a custom event to the data layer: ssRawProductDetailPush. 
When the quick view pops up, there’s a CSS selector associated with it (.sqs-product-quick-view-content) that I use as my trigger.

## select_item: When a user selects a product from a list of items or offerings 
[Get the custom code here in my repo.](ga4-ecommerce-select_item.html)

### Trigger: 
**Trigger Type**: Page View - Window Loaded
**This trigger fires on**: Page Path equals /store

💡My custom code attaches a click event listener to every product block on the /store page. Since I’m using an event listener, I need to use the window loaded trigger to ensure the HTML elements were fully loaded on the page before firing the tag. This is is especially critical for tags where we attach event listeners to elements that might be rendered after the initial page load.

## add_to_cart: When a user clicks on the add to cart button

I needed to create two separate tags for the add_to_cart event: one for when someone clicks on the adds to cart button on the store page and when someone clicks on the increment button on the cart page.

<details>
  <summary>## Click on the Add to Cart Button</summary>
  [Get the custom code here in my repo.](ga4-ecommerce-add_to_cart-clicked-on-add-to-cart.html)
 
  ### Trigger: 
**Trigger Type**: Click - All Elements
**This trigger fires on**: Click Text contains ADD TO CART

💡You can change the click text if you’re website uses different text.
</details>

<details>
  <summary>## Click on the Increment Button</summary>
  [Get the custom code here in my repo.](ga4-ecommerce-add_to_cart-clicked-on-increment-button.html)
 
  ### Trigger: 
**Trigger Type**: Click - All Elements
**This trigger fires on**: Page Path contains /cart
  Trigger Type: Page View - Window Loaded

💡I chose the Window Loaded trigger type to ensure the HTML elements were fully loaded on the page before firing the tag. This is is especially critical for tags where we attach event listeners to elements that might be rendered after the initial page load.
</details>

## view_cart: When a user views their shopping cart
[Get the custom code here in my repo.](ga4-ecommerce-view_cart.html)

### Trigger: 
**Trigger Type**: Page View - Window Loaded
**This trigger fires on**: Page Path contains /cart 

💡I chose the Window Loaded trigger type to ensure the HTML elements were fully loaded on the page before firing the tag. This is is especially critical for tags where we attach event listeners to elements that might be rendered after the initial page load.

##  remove_from_cart: When a user removes items from their shopping cart
[Get the custom code here in my repo.](ga4-ecommerce-remove_from_cart.htm)

### Trigger: 
**Trigger Type**: Page View - Window Loaded
**This trigger fires on**: Page Path contains /cart

💡I chose the Window Loaded trigger type to ensure the HTML elements were fully loaded on the page before firing the tag. This is is especially critical for tags where we attach event listeners to elements that might be rendered after the initial page load.

## begin_checkout: When a user begins checkout
[Get the custom code here in my repo.](ga4-ecommerce-begin_checkout.html)

 ### Trigger: 
**Trigger Type**: Click - All Elements
**This trigger fires on**: Click Classes contains cart-checkout-button

💡This code takes advantage of the JSON Squarespace creates out of the box for us. There’s actually JSON on every ecommerce page, but we can’t use it because its static on page load and doesn’t change if the user changes anything about the product or cart after the initial page load. 

## purchase: When a user begins checkout
[Get the custom code here in my repo.](ga4-ecommerce-purchase.html)

### Trigger: 
**Trigger Type**: Page View
**This trigger fires on**: Page Path contains /commerce/orders/

## Summary

Squarespace doesn’t make it easy to track all of your ecommerce data in GA4, but I’m hoping this guide helps you properly set it up in Google Tag Manager.
Custom HTML code to push ecommerce data into the data layer for GA4 ecommerce tracking on a Squarespace website
