<diapoComponent v-bind="{title:'Mission',reference:'20'}">
    <p>Mission 1/3 : GRUD des Patients</p>
    <i>()</i>
</diapoComponent>
```php{*}{class:' children:h-100'}
    public function index(Request $request): JsonResponse
    {
        $patients = [];
        $currentUser = $this->jwtSecurity->getUser();
        if(!$this->jwtSecurity->isGranted('ROLE_ADMIN', $currentUser))
        {
            foreach($currentUser->getPatients() as $patient)
            {
                array_push($patients,$patient->getVisible());
            }

            return new JsonResponse(['patients' => $patients]);
        }

        $filter = [
            'firstName' => strip_tags($request->query->get('firstName')??""),
            'lastName' => strip_tags($request->query->get('lastName')??""),
        ]
        ;
        $pages= intval(strip_tags($request->query->get('pages')??'0'));
        if($filter['firstName'] !== "" || $filter["lastName"] !== "")
        {
            $patients = $this->patientRepository->findByFilter($filter);

            return new JsonResponse(['patients' => $patients]);
        }

        $patients = $this->patientRepository->getAll($pages);


        return new JsonResponse(['patients' => $patients],status: 200);
    }
```
