<PagesComponent/>
<ReferenceComponent>8-10</ReferenceComponent>
<div>
    <h1>Mission</h1>
    <p>Alerte commande 0€ et commande à 0€</p>
</div>
<i>(apps/sylius/config/state_machine_override/sylius_order_checkout.yaml)</i>
````md magic-move v-click.hide
```yaml
    skip_payment:
        from: [shipping_selected, shipping_skipped, health_profile_completed, health_profile_skipped]
        to: completed
    complete:
        from: [shipping_selected, shipping_skipped]
```
```yaml
    skip_payment:
        from: [shipping_selected, shipping_skipped]
        to: completed
    complete:
        from: [shipping_selected, shipping_skipped]
```
````
<i>(apps/sylius/src/Shop/EmailManager/Order/OrderEmailManager.php)</i>
```php
        if (0 === $order->getTotal()) {
            $this->sendConfirmationEmailToAdmin($order);
        }
```
