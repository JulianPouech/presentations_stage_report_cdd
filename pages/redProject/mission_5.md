<diapoComponent v-bind="{title:'Mission',reference:'20'}">
</diapoComponent>
```php{*}{class:' children:h-90'}
    public function delete(int $id): JsonResponse
    {
        if(!$this->jwtSecurity->isGranted('ROLE_ADMIN'))
        {
            return new JsonResponse(status: 403);
        }

        /** @var ?Patient */
        $patient = $this->patientRepository->findOneBy(['id' => $id]);

        if(!$patient instanceof Patient)
        {
            return new JsonResponse(status:  '404');
        }

        $patient->setFirstName('firstName'.$patient->getId());
        $patient->setLastName('lastName'.$patient->getId());
        $patient->setPhone('');
        $patient->getAddress()->setAddress('address'.$patient->getId());
        $patient->getAddress()->setCountry('');
        $patient->getAddress()->setPostalCode('');
        $patient->getAddress()->setCity('');

        foreach($patient->getUsers() as $user)
        {
            $user->removePatient($patient);
            $this->entityManager->persist($user);
        };

        $patient->clearUsers();
        $this->patientRepository->update($patient);

        return new JsonResponse(status: 200);
    }
```
