<PagesComponent/>
<ReferenceComponent>11-13</ReferenceComponent>
<div>
    <h1>Mission</h1>
    <p>Liens "Nos marques + top catégorie" manquants</p>
</div>

````md magic-move {class:'overflow-auto max-h-100'}
```php
    public function findByTaxon(Taxon $taxon): array
    {
           return $this->createQueryBuilder('b')
            ->innerJoin(Product::class, 'p', 'WITH', 'b = p.brand')
            ->andWhere($this->_em->getExpressionBuilder()->orX(
                'p.mainTaxon = :taxon',
                ':taxon MEMBER OF p.productTaxons',
            ))
            ->andWhere('b.enabled = true')
            ->setParameter('taxon', $taxon)
            ->orderBy('b.name', 'ASC')
            ->getQuery()
            ->getResult()
        ;
    }
```
```php
    public function findByTaxon(Taxon $taxon, bool $includeAllDescendants = true): array
    {
        if ($includeAllDescendants) {
            $taxonIds = $this->_em
                ->createQueryBuilder()
                ->from(Taxon::class, 't')
                ->select('t.id')
                ->where('t.left >= :left')
                ->andWhere('t.right <= :right')
                ->setParameter('left', $taxon->getLeft())
                ->setParameter('right', $taxon->getRight())
                ->getQuery()
                ->getSingleColumnResult()
            ;
        }
            return $this->createQueryBuilder('b')
                ->innerJoin(Product::class, 'p', 'WITH', 'b = p.brand')
                    ->leftJoin('p.productTaxons', 'pt')
                ->andWhere($this->_em->getExpressionBuilder()->orX(
                    'p.mainTaxon IN (:taxons)',
                    'pt.taxon IN (:taxons)',
                ))
                ->andWhere('b.enabled = true')
                ->setParameter('taxons', $taxonIds)
                ->orderBy('b.name', 'ASC')
                ->getQuery()
                ->getResult()
            ;
    }
```
````
