<PagesComponent/>
<ReferenceComponent>2-3</ReferenceComponent>
<div>
    <h1>Mission</h1>
    <p>Mission 1/3 : GRUD des Patients</p>
    <i>(src/Entity/Patient.php)</i>
</div>

```php{*}{class:' children:h-90'}
    public function getVisible(): array
    {
        return [
            'id' => $this->id,
            'firstName' => $this->firstName,
            'lastName' => $this->lastName,
            'address' => $this->address,
            'phone' => $this->phone,
        ];
    }

    public function getId():?int {
        return $this->id;
    }

    public function setId(?int $id):void {
        $this->id = $id;
    }

    public function getFirstName():?string {
        return $this->firstName;
    }

    public function setFirstName(?string $firstName): void {
        $this->firstName = $firstName;
    }

    public function getLastName():?string {
        return $this->lastName;
    }

    public function setLastName(?string $lastName): void {
        $this->lastName = $lastName;
    }

    public function getPhone():?string {
        return $this->phone;
    }

    public function setPhone(?string $phone): void {
        $this->phone = $phone;
    }
```
