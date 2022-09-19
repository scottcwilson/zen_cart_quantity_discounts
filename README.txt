Quantity Discounts Module for Zen Cart 
Version 2.1 
--------------------------------------------------
Released under the GNU General Public License
--------------------------------------------------
Author: Scott Wilson
https://www.thatsoftwareguy.com 
Zen Forum PM swguy
Donations welcome at https://donate.thatsoftwareguy.com/

Even more information on this contribution is provided in

https://www.thatsoftwareguy.com/zencart_quantity_discounts.html

PLEASE READ THIS PAGE PRIOR TO POSTING A QUESTION TO THE FORUM.

No warranties expressed or implied; use at your own risk.

--------------------------------------------------
Overview: 
This mod permits a shop to define up to five (admin specified) discount levels
on a per-product, per category or total number of items purchased basis.  
Discounts may also be done on the basis of dollars spent.
The discounts may be expressed in percentage or currency units.  Various
user exits are provided to make it easy to extend the mod to meet 
shop requirements.

This limit of 5 levels can be exceeded by specifying your own levels.

Detailed Description: 

a) Configuration
Discount Level 1..5 and Discount 1..5 are fields which contain
a set of five discount levels (min. quantity required to get to this 
level, discount amount) may be specified.   

The Discount Basis radio buttons determine if quantity required to 
get to a discount level is counted as bulk purchases of a single item 
(Total By Item), by purchases within a category (Total By Category) 
or by the total number of items in the cart (Total Items in Cart).   

The Discount Units radio buttons allow the units of discount levels to 
be specified as percent off, or currency units off (e.g. dollars off total),
or as currency units per item off (e.g. dollars off per item).

Note that when using Total By Category for products in subcategories, 
the "category" is the parent subcategory, which will not be the 
same as the top level category for items which are in subcategories.  

Specifying "include tax = true" will gross the discount up by the amount
of tax that would have been paid on the goods pre-discount.  This is 
appropriate for shops under a price inclusive sales tax system, such as 
the UK VAT.   Specifying Re-calculate tax = Standard will recalculate the
entire tax based on the original subtotal minus the quantity discount.

Counting method allows you to discount by dollars spent (total in cart, by
item or by category), as opposed to the typical way of determining totals,
which is by count of items.

b) User Exits

Some shops will want to further customize their discounting policy.

Additional user exits are provided to permit the following customizations: 
 * Include or exclude certain categories from discounting.
 * Include or exclude certain products from discounting. 
 * Modify the discounts for particular categories if discounting 
   by category.
 * Modify the discounts for particular items if discounting by item.

Specifically: 
 * The function exclude_category() shows you how to add categories to 
a list of categories to not be discounted.  This may be used with any
Discount Basis.

 * The function exclude_product() shows you how to add products to 
a list of products to not be discounted.  This may be used with any 
Discount Basis.

  * If you are using the Discount Basis "Total By Category," you may 
customize the discount for a specific category using the function 
apply_special_category_discount().  

  * If you are using the Discount Basis "Total By Item," you may 
customize the discount for a specific item using the function 
apply_special_item_discount().  

Each of these functions contains a simple example of how to use it using
item and category numbers like 99999, 99998, etc.

Quantity Discounts discounts off the *gross* price of the goods.  
This can result in a larger than expected discount if other discounts
are added (such as coupons or group discounts), so bear this in mind 
when creating your discount strategy.

If you need help adding custom logic, please go to www.thatsoftwareguy.com
and send me email.  There is a fee for this service based on the 
complexity of the requested change.

c) Breaking the Five Level Barrier

If you require more than five levels of discounts, look in the
file includes/modules/order_total/ot_quantity_discount.php at the 
function called setup().  This provides an example of how to 
add more level, discount pairs.  

--------------------------------------------------

Installation Instructions: 
0. Back up everything!  Try this in a test environment prior to installing
it on a live shop.

1. If you already have the Quantity Discounts module installed, please 
deinstall your old copy by going to Admin->Modules->Order Total, 
selecting "Quantity Discount" and pressing the "Remove" button.  Make
a note of your settings so you can apply them to the new version.

If you have customized the Quantity Discounts code 
(ot_quantity_discounts.php) to include or exclude particular 
categories or modify discount levels, back up this file.

2. Copy the contents of the unzipped folder to the root directory of your
shop.  There are two new files in this mod.  Previous versions of this
mod required changes to shopping_cart.php; this is no longer true after
ZenCart 1.3.5.

3. Login to admin and in Modules->Order Total you will see 'Quantity Discount' listed along with all the other modules available.

4. Click on 'Quantity Discount' to highlight the module and click on 'Install'

5. Decide on the parameters you wish to use.  The easiest way to do this
is to open a shopping cart in another window, and just try different 
discounting models.  The discounts are shown on the second step in 
"Your Total" under "Quantity Discount," which is itself a link that
explains the calculation.

6. Customization: If you have a single discounting policy for your shop,
you're all set.  If you wish to tailor the policy, you will have to 
add code to the user exits as described above.

If you need help doing this work, please email me from my home page at 
www.thatsoftwareguy.com

7. If you wish, get a copy of the Quantity Discounts Promotional Page by 
following the links on 

https://www.thatsoftwareguy.com/zencart_quantity_discounts.html

The Quantity Discounts Promotional Page displays your Quantity Discount policy.

Alternately, you may build your own promotional section on an existing page,
such as an "about us" page or some other page that describes your 
policies, using this code: 

<?php
  $value = "ot_quantity_discount.php"; 
  include(zen_get_file_directory(DIR_WS_LANGUAGES . $_SESSION['language'] . 
          '/modules/order_total/', $value, 'false'));
  include(DIR_WS_MODULES . "order_total/" . $value);
  $discount = new ot_quantity_discount();  
  if ($discount->check() > 0) {
     echo '<div class="content" id="discountPolicy">'; 
     echo '<h2>' . STORE_POLICY . '</h2>'; 
     echo $discount->get_html_policy();
     echo '<br /></div>'; 
  }
?>

Notes: 

* You may also put this into tpl_product_info_display.php if you
wish to advertise your discounts there.  See the many examples on 
my web page for retrieving and formatting this information.

* If you wish to format this information yourself, please use the 
get_discount_info() function, which returns an array of strings.

* If you have used the apply_special_category_discount() or 
apply_special_item_discount() exits, you should add text
below the call to get_html_policy() to describe these special cases. 

* If you are also using the Discount Preview Extension, please be sure that
the second and third statements above in your code are "include_once" and not
"include" as they were formerly.

New Files
=========
includes/languages/english/modules/order_total/ot_quantity_discount.php
includes/modules/order_total/ot_quantity_discount.php

