<div>
    <h1></h1>
    <i>(migrations/Version20250124160151.php)</i>
</div>

```php{*}{class:' children:h-90'}
    public function up(Schema $schema): void
    {
        // this up() migration is auto-generated, please modify it to your needs
        $this->addSql('CREATE TABLE patients (
            id SERIAL,
            address_id INT,
            first_name VARCHAR(128) NOT NULL,
            last_name VARCHAR(128) NOT NULL,
            phone VARCHAR(16),
            PRIMARY KEY(id),
            CONSTRAINT fk_address
                    FOREIGN KEY (address_id) REFERENCES "address" (id) ON DELETE CASCADE NOT DEFERRABLE INITIALLY IMMEDIATE
            )');
        $this->addSql('CREATE TABLE users_patients (
            user_id INT NOT NULL,
            patient_id INT NOT NULL,
            PRIMARY KEY(user_id, patient_id),
            CONSTRAINT fk_user
                FOREIGN KEY (user_id) REFERENCES "user" (id) ON DELETE CASCADE NOT DEFERRABLE INITIALLY IMMEDIATE,
            CONSTRAINT fk_patient
                FOREIGN KEY (patient_id) REFERENCES patients (id) ON DELETE CASCADE NOT DEFERRABLE INITIALLY IMMEDIATE
        )');
    }

    public function down(Schema $schema): void
    {
        // this down() migration is auto-generated, please modify it to your needs
        $this->addSql('DROP TABLE users_patients');
        $this->addSql('DROP TABLE patients');
    }
```

