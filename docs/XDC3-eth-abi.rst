.. _eth-abi:

.. include:: include_announcement.rst

=========
xdc3.eth.abi
=========

The ``xdc3-eth-abi`` package allows you to de- and encode parameters from a ABI (Application Binary Interface).
This will be used for function calls to the EVM (Ethereum Virtual Machine).

.. code-block:: javascript

    import {AbiCoder} from 'xdc3-eth-abi';

    const abiCoder = new AbiCoder();


    // or using the xdc3 umbrella package


    import {xdc3} from 'xdc3';

    const xdc3 = new xdc3(xdc3.givenProvider || 'ws://some.local-or-remote.node:8546', options);
    // -> xdc3.eth.abi




------------------------------------------------------------------------------


encodeFunctionSignature
=====================

.. code-block:: javascript

    xdc3.eth.abi.encodeFunctionSignature(functionName);

Encodes the function name to its ABI signature, which are the first 4 bytes of the sha3 hash of the function name including types.

----------
Parameters
----------

1. ``functionName`` - ``String|Object``: The function name to encode.
or the :ref:`JSON interface <glossary-json-interface>` object of the function. If string it has to be in the form ``function(type,type,...)``, e.g: ``myFunction(uint256,uint32[],bytes10,bytes)``

-------
Returns
-------

``String`` - The ABI signature of the function.

-------
Example
-------

.. code-block:: javascript

    // From a JSON interface object
    xdc3.eth.abi.encodeFunctionSignature({
        name: 'myMethod',
        type: 'function',
        inputs: [{
            type: 'uint256',
            name: 'myNumber'
        },{
            type: 'string',
            name: 'myString'
        }]
    })
    > 0x24ee0097

    // Or string
    xdc3.eth.abi.encodeFunctionSignature('myMethod(uint256,string)')
    > '0x24ee0097'


------------------------------------------------------------------------------

encodeEventSignature
=====================

.. code-block:: javascript

    xdc3.eth.abi.encodeEventSignature(eventName);

Encodes the event name to its ABI signature, which are the sha3 hash of the event name including input types.

----------
Parameters
----------

1. ``eventName`` - ``String|Object``: The event name to encode.
or the :ref:`JSON interface <glossary-json-interface>` object of the event. If string it has to be in the form ``event(type,type,...)``, e.g: ``myEvent(uint256,uint32[],bytes10,bytes)``

-------
Returns
-------

``String`` - The ABI signature of the event.

-------
Example
-------

.. code-block:: javascript

    xdc3.eth.abi.encodeEventSignature('myEvent(uint256,bytes32)')
    > 0xf2eeb729e636a8cb783be044acf6b7b1e2c5863735b60d6daae84c366ee87d97

    // or from a json interface object
    xdc3.eth.abi.encodeEventSignature({
        name: 'myEvent',
        type: 'event',
        inputs: [{
            type: 'uint256',
            name: 'myNumber'
        },{
            type: 'bytes32',
            name: 'myBytes'
        }]
    })
    > 0xf2eeb729e636a8cb783be044acf6b7b1e2c5863735b60d6daae84c366ee87d97


------------------------------------------------------------------------------

encodeParameter
=====================

.. code-block:: javascript

    xdc3.eth.abi.encodeParameter(type, parameter);

Encodes a parameter based on its type to its ABI representation.

----------
Parameters
----------

1. ``type`` - ``String|Object``: The type of the parameter, see the `solidity documentation <http://solidity.readthedocs.io/en/develop/types.html>`_  for a list of types.
2. ``parameter`` - ``Mixed``: The actual parameter to encode.

-------
Returns
-------

``String`` - The ABI encoded parameter.

-------
Example
-------

.. code-block:: javascript

    xdc3.eth.abi.encodeParameter('uint256', '2345675643');
    > "0x000000000000000000000000000000000000000000000000000000008bd02b7b"

    xdc3.eth.abi.encodeParameter('uint256', '2345675643');
    > "0x000000000000000000000000000000000000000000000000000000008bd02b7b"

    xdc3.eth.abi.encodeParameter('bytes32', '0xdf3234');
    > "0xdf32340000000000000000000000000000000000000000000000000000000000"

    xdc3.eth.abi.encodeParameter('bytes', '0xdf3234');
    > "0x00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000003df32340000000000000000000000000000000000000000000000000000000000"

    xdc3.eth.abi.encodeParameter('bytes32[]', ['0xdf3234', '0xfdfd']);
    > "00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000002df32340000000000000000000000000000000000000000000000000000000000fdfd000000000000000000000000000000000000000000000000000000000000"



------------------------------------------------------------------------------

encodeParameters
=====================

.. code-block:: javascript

    xdc3.eth.abi.encodeParameters(typesArray, parameters);

Encodes a function parameters based on its :ref:`JSON interface <glossary-json-interface>` object.

----------
Parameters
----------

1. ``typesArray`` - ``Array<String|Object>|Object``: An array with types or a :ref:`JSON interface <glossary-json-interface>` of a function. See the `solidity documentation <http://solidity.readthedocs.io/en/develop/types.html>`_  for a list of types.
2. ``parameters`` - ``Array``: The parameters to encode.

-------
Returns
-------

``String`` - The ABI encoded parameters.

-------
Example
-------

.. code-block:: javascript

    xdc3.eth.abi.encodeParameters(['uint256','string'], ['2345675643', 'Hello!%']);
    > "0x000000000000000000000000000000000000000000000000000000008bd02b7b0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000000748656c6c6f212500000000000000000000000000000000000000000000000000"

    xdc3.eth.abi.encodeParameters(['uint8[]','bytes32'], [['34','434'], '0x324567fff']);
    > "0x0"


------------------------------------------------------------------------------

encodeFunctionCall
=====================

.. code-block:: javascript

    xdc3.eth.abi.encodeFunctionCall(jsonInterface, parameters);

Encodes a function call using its :ref:`JSON interface <glossary-json-interface>` object and given paramaters.

----------
Parameters
----------

1. ``jsonInterface`` - ``Object``: The :ref:`JSON interface <glossary-json-interface>` object of a function.
2. ``parameters`` - ``Array``: The parameters to encode.

-------
Returns
-------

``String`` - The ABI encoded function call. Means function signature + parameters.

-------
Example
-------

.. code-block:: javascript

    xdc3.eth.abi.encodeFunctionCall({
        name: 'myMethod',
        type: 'function',
        inputs: [{
            type: 'uint256',
            name: 'myNumber'
        },{
            type: 'string',
            name: 'myString'
        }]
    }, ['2345675643', 'Hello!%']);
    > "0x24ee0097000000000000000000000000000000000000000000000000000000008bd02b7b0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000000748656c6c6f212500000000000000000000000000000000000000000000000000"

------------------------------------------------------------------------------

decodeParameter
=====================

.. code-block:: javascript

    xdc3.eth.abi.decodeParameter(type, hexString);

Decodes an ABI encoded parameter to its JavaScript type.

----------
Parameters
----------

1. ``type`` - ``String|Object``: The type of the parameter, see the `solidity documentation <http://solidity.readthedocs.io/en/develop/types.html>`_  for a list of types.
2. ``hexString`` - ``String``: The ABI byte code to decode.

-------
Returns
-------

``Mixed`` - The decoded parameter.

-------
Example
-------

.. code-block:: javascript

    xdc3.eth.abi.decodeParameter('uint256', '0x0000000000000000000000000000000000000000000000000000000000000010');
    > "16"

    xdc3.eth.abi.decodeParameter('string', '0x0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000848656c6c6f212521000000000000000000000000000000000000000000000000');
    > "Hello!%!"

    xdc3.eth.abi.decodeParameter('string', '0x0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000848656c6c6f212521000000000000000000000000000000000000000000000000');
    > "Hello!%!"


------------------------------------------------------------------------------

decodeParameters
=====================

.. code-block:: javascript

    xdc3.eth.abi.decodeParameters(typesArray, hexString);

Decodes ABI encoded parameters to its JavaScript types.

----------
Parameters
----------

1. ``typesArray`` - ``Array<String|Object>|Object``: An array with types or a :ref:`JSON interface <glossary-json-interface>` outputs array. See the `solidity documentation <http://solidity.readthedocs.io/en/develop/types.html>`_  for a list of types.
2. ``hexString`` - ``String``: The ABI byte code to decode.

-------
Returns
-------

``Object`` - The result object containing the decoded parameters.

-------
Example
-------

.. code-block:: javascript

    xdc3.eth.abi.decodeParameters(['string', 'uint256'], '0x000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000ea000000000000000000000000000000000000000000000000000000000000000848656c6c6f212521000000000000000000000000000000000000000000000000');
    > Result { '0': 'Hello!%!', '1': '234' }

    xdc3.eth.abi.decodeParameters([{
        type: 'string',
        name: 'myString'
    },{
        type: 'uint256',
        name: 'myNumber'
    }], '0x000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000ea000000000000000000000000000000000000000000000000000000000000000848656c6c6f212521000000000000000000000000000000000000000000000000');
    > Result {
        '0': 'Hello!%!',
        '1': '234',
        myString: 'Hello!%!',
        myNumber: '234'
    }


------------------------------------------------------------------------------


decodeLog
=====================

.. code-block:: javascript

    xdc3.eth.abi.decodeLog(inputs, hexString, topics);

Decodes ABI encoded log data and indexed topic data.

----------
Parameters
----------

1. ``inputs`` - ``Array``: A :ref:`JSON interface <glossary-json-interface>` inputs array. See the `solidity documentation <http://solidity.readthedocs.io/en/develop/types.html>`_  for a list of types.
2. ``hexString`` - ``String``: The ABI byte code in the ``data`` field of a log.
3. ``topics`` - ``Array``: An array with the index parameter topics of the log, without the topic[0] if its a non-anonymous event, otherwise with topic[0].

-------
Returns
-------

``Object`` - The result object containing the decoded parameters.

-------
Example
-------

.. code-block:: javascript


    xdc3.eth.abi.decodeLog([{
        type: 'string',
        name: 'myString'
    },{
        type: 'uint256',
        name: 'myNumber',
        indexed: true
    },{
        type: 'uint8',
        name: 'mySmallNumber',
        indexed: true
    }],
    '0x0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000748656c6c6f252100000000000000000000000000000000000000000000000000',
    ['0x000000000000000000000000000000000000000000000000000000000000f310', '0x0000000000000000000000000000000000000000000000000000000000000010']);
    > Result {
        '0': 'Hello%!',
        '1': '62224',
        '2': '16',
        myString: 'Hello%!',
        myNumber: '62224',
        mySmallNumber: '16'
    }
