Associative Array
=================

Pre-existent Type
-----------------

+------------------------------------------------+-----------------+
|                  Php Type                      |   Value Type    |
+================================================+=================+
| BeSimple\\SoapCommon\\Type\\KeyValue\\StringType   | String          |
+------------------------------------------------+-----------------+
| BeSimple\\SoapCommon\\Type\\KeyValue\\BooleanType  | Boolean         |
+------------------------------------------------+-----------------+
| BeSimple\\SoapCommon\\Type\\KeyValue\\IntType      | Int             |
+------------------------------------------------+-----------------+
| BeSimple\\SoapCommon\\Type\\KeyValue\\FloatType    | Float           |
+------------------------------------------------+-----------------+
| BeSimple\\SoapCommon\\Type\\KeyValue\\DateType     | DateTime object |
+------------------------------------------------+-----------------+
| BeSimple\\SoapCommon\\Type\\KeyValue\\DateTimeType | DateTime object |
+------------------------------------------------+-----------------+

Controller
----------

.. code-block:: php

    namespace Acme\DemoBundle\Controller;

    use BeSimple\SoapBundle\ServiceDefinition\Annotation as Soap;
    use Symfony\Component\DependencyInjection\ContainerAwareInterface;
    use Symfony\Component\DependencyInjection\ContainerAwareTrait;

    class DemoController  implements ContainerAwareInterface
    {
        use ContainerAwareTrait;

        /**
         * @Soap\Method("returnAssocArray")
         * @Soap\Result(phpType = "BeSimple\SoapCommon\Type\KeyValue\StringType[]")
         */
        public function assocArrayOfStringAction()
        {
            return array(
                'foo' => 'bar',
                'bar' => 'foo',
            );
        }

        /**
         * @Soap\Method("sendAssocArray")
         * @Soap\Param("assocArray", phpType = "BeSimple\SoapCommon\Type\KeyValue\StringType[]")
         * @Soap\Result(phpType = "BeSimple\SoapCommon\Type\KeyValue\StringType[]")
         */
        public function sendAssocArrayOfStringAction(array $assocArray)
        {
            // The $assocArray it's a real associative array
            // var_dump($assocArray);die;

            return $assocArray;
        }
    }

How to create my Associative Array?
-----------------------------------

.. code-block:: php

    namespace Acme\DemoBundle\Soap\Type;

    use BeSimple\SoapBundle\ServiceDefinition\Annotation as Soap;
    use BeSimple\SoapCommon\Type\AbstractKeyValue;

    class User extends AbstractKeyValue
    {
        /**
         * @Soap\ComplexType("Acme\DemoBundle\Entity\User")
         */
        protected $value;
    }

.. code-block:: php

    namespace Acme\DemoBundle\Controller;

    use Acme\DemoBundle\Entity\User;
    use BeSimple\SoapBundle\ServiceDefinition\Annotation as Soap;
    use Symfony\Component\DependencyInjection\ContainerAwareInterface;
    use Symfony\Component\DependencyInjection\ContainerAwareTrait;

    class DemoController  implements ContainerAwareInterface
    {
        use ContainerAwareTrait;

        /**
         * @Soap\Method("getUsers")
         * @Soap\Result(phpType = "Acme\DemoBundle\Soap\Type\User[]")
         */
        public function getUsers()
        {
            return array(
                'user1' => new User('user1', 'user1@user.com'),
                'user2' => new User('user2', 'user2@user.com'),
            );
        }
    }
