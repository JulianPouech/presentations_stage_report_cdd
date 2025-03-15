<div>
    <h1>Mis En place de la BDD</h1>
    <i>(migrations/Version20250109130407.php)</i>
</div>

```php{*}{class:' children:h-90'}
    public function up(Schema $schema): void
    {
        // this up() migration is auto-generated, please modify it to your needs
        $this->addSql('
            CREATE TABLE address(
                id SERIAL,
                address VARCHAR(255) NOT NULL,
                city VARCHAR(255) NOT NULL,
                postal_code VARCHAR(16) NOT NULL,
                country VARCHAR(2) DEFAULT NULL,
                PRIMARY KEY(id))'
        );

        $this->addSql('
            CREATE TABLE "user"(
                id SERIAL,
                address_id INT DEFAULT NULL,
                email VARCHAR(180) NOT NULL,
                roles JSON NOT NULL,
                password VARCHAR(255) NOT NULL, PRIMARY KEY(id),
                CONSTRAINT fk_address
                    FOREIGN KEY(address_id)
                    REFERENCES address(id))
        ');
    }

    public function down(Schema $schema): void
    {
        // this down() migration is auto-generated, please modify it to your needs
        $this->addSql('DROP TABLE "user"');
        $this->addSql('DROP TABLE address');
    }
```

