#  Term - Symfony forms 

**[Symfony forms](http://symfony.com/doc/current/forms.html)** that are handled in eZ Commerce consists by default of two parts:

  - Form entity
  - Form type

*Form entity* is just a simple class that holds the data and contains the [validation](https://symfony.com/doc/current/reference/constraints.html) annotations.

[*Form type*](https://symfony.com/doc/current/reference/forms/types.html#main) (usually defined as a service) builds the form. Here is defined which form attributes are rendered and how: e.g. as an input field, or textarea, custom attributes (css classes, data attributes) can be set here, you can define the labels or validation groups and much more.

The form itself is than build by the appropriate instance like that:

**Example**

``` 
class FormController extends Controller
{
    public function createFormAction()
    {
        $formType = $this->get('test_form_type_service');
        $formEntity = new TestForm();       
        
        $form = $this->createForm($formType, $formEntity);

        return $this->render('ProjectTestBundle:Form:create_form.html.twig', array('form' => $form->createView()));
    }
}
```
