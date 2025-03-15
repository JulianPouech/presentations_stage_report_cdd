<diapoComponent :title="'GRUD des Patients en BO
'" :ref="''">
    <i>(src/Entity/Patient.php)</i>
</diapoComponent>

```php{*}{class:' children:h-90'}
    #[ORM\Entity]
    #[ORM\Table(name: 'patients')]
    class Patient implements EntityInterface
    {
        #[ORM\Id]
        #[ORM\GeneratedValue]
        #[ORM\Column]
        private ?int $id = null;

        #[ORM\Column(type: Types::STRING,length:128)]
        private ?string $firstName = null;

        #[ORM\Column(type: Types::STRING,length:128)]
        private ?string $lastName = null;

        #[ORM\Column(type: Types::STRING,length:16,nullable: true)]
        private ?string $phone = null;

        /**
         * @var Collection<int, User>
         */
        #[ORM\ManyToMany(targetEntity: User::class, mappedBy: 'patients')]
        private Collection $users;

        #[ORM\OneToOne(cascade: ['persist', 'remove'])]
        private ?Address $address = null;


        public function __construct() {
            $this->users = new ArrayCollection();
        }

```
