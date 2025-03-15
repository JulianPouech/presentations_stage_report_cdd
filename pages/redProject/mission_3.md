<diapoComponent reference="'10'">
        <i></i>
</diapoComponent>

```php{*}{class:' children:h-90'}
class PatientController implements ControllerInterface
{
    use TraitErrorForm;

    public function __construct(private PatientRepository $patientRepository,
        private FormFactoryInterface $formFactory,
        private JwtSecurity $jwtSecurity,
        private EntityManagerInterface $entityManager
    ) {
    }

    public function update(Request $request, int $id): JsonResponse
    {
        $patient = null;
        $currentUser = $this->jwtSecurity->getUser();

        if($this->jwtSecurity->isGranted('ROLE_ADMIN'))
        {
            $patient = $this->patientRepository->find(['id' => $id]);
        }else{
            $patient = $this->patientRepository->findByUserId($currentUser->getId(), $id);
        }

        if(!$patient instanceof Patient)
        {
            return new JsonResponse(status: 403);
        }

        $payload = json_decode(strip_tags($request->getContent()), true);
        $form = $this->formFactory->create(PatientType::class, $patient);
        $form->submit($payload);

        if(!$form->isValid())
        {
            return new JsonResponse($this->errorsFormToJson($form), status:406);
        }

        $this->patientRepository->update($patient);

        return new JsonResponse(status: 200);
    }
```
