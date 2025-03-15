<PagesComponent/>
<ReferenceComponent>8-10</ReferenceComponent>
<div>
    <h1> Mission </h1>
    <p>Alerte commande 0€ et commande à 0€</p>
</div>
<i>apps/sylius/src/Shop/EmailManager/Order/OrderEmailManager.php</i>
```php{all}{class:' children:h-90'}
    public function sendConfirmationEmailToAdmin(OrderInterface $order, ?string $subject = null): void
    {
        if (null === $subject) {
            $subject = 'Commande à 0€ - N°' . $order->getNumber();
        }

        $channel = $order->getChannel();

        $locale = $order->getLocaleCode() ?? 'fr';

        $adminMail = (new TemplatedEmail())
            ->to('contact@...')
            ->from("{$channel->getName()} <{$this->noreplyEmail}>")
            ->subject($subject)
            ->htmlTemplate('emails/shop/order/orderConfirmation.html.twig')
            ->context([
                'locale' => $locale,
                'channel' => $channel,
                'order' => $order,
            ])
        ;

        if (!$adminMail instanceof TemplatedEmail) {
            return;
        }

        try {
            $this->mailer->send($adminMail);
        } catch (TransportExceptionInterface|HandlerFailedException $exception) {
            $this->mailLogger->critical('Send admin mail confirmation order, failed for orderId : ' . $order->getId() . ' with message : ' . $exception->getMessage(), ['trace' => $exception->getTraceAsString()]);

            throw $exception;
        }
    }
```


