MicroPython Home Assistant
==========================

Communicate with your Home Assistant instance from MicroPython.

Expects `micropython http-client <https://github.com/balloob/micropython-http-client>`_ in the same directory.

Currently supported methods:

- ``states()``
- ``is_state(entity_id, state)``
- ``get_state(entity_id)``
- ``set_state(entity_id, state, attributes=None)``
- ``fire_event(event_name, event_data=None)``
- ``call_service(domain, service, service_data=None)``

.. code-block:: python

    hass = HomeAssistant('http://127.0.0.1:8123')
    states = hass.states()
    state = states[0]
    print("State %s is %s" % (state['entity_id'], state['state']))
    print("Test if state is still the same: %s" %
          hass.is_state(state['entity_id'], state['state']))

    new_state = hass.set_state('sensor.temperature', '10',
                               {'unit_of_measurement': '%'})
    verify_state = hass.get_state('sensor.temperature')
    print(new_state)
    print(verify_state)
    print(new_state == verify_state)

    hass.fire_event('some_event', {'hello': 'world'})

    hass.call_service('switch', 'turn_on', {'entity_id': 'switch.ac'})

Notes
-----

- SSL certificates are not being verified.
- Password protected Home Assistant instances are not yet supported.
- Not all micropython implementations support timeout. It defaults to 5 seconds
  if supported. It can be overwritten by passing in a second argument to the
  ``HomeAssistant`` constructor.