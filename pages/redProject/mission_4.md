<diapoComponent :title="'Mission'" :ref="''">
</diapoComponent>
```php{*}{class:' children:h-90'}
    public function create(Request $request): JsonResponse
    {
        $payload = json_decode(strip_tags($request->getContent()), true);

        $patient = new Patient();
        $form = $this->formFactory->create(PatientType::class, $patient);
        $form->submit($payload);

        if(!$form->isValid()){
            return new JsonResponse($this->errorsFormToJson($form), status:406);
        }

        $patient->addUser($this->jwtSecurity->getUser());
        $this->patientRepository->update($patient);

        return new JsonResponse(status: 201);
    }
```
