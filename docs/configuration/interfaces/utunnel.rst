:lastproofread: 2024-12-12

.. _custom-tunnel-interface:

Custom Tunnel
=============

This article touches on custom tunnel devices.

All those protocols are grouped under ``interfaces utunnel`` in VyOS. Let's take
a closer look at the protocols and options currently supported by VyOS.

Common interface configuration
------------------------------

.. cmdinclude:: /_include/interface-address.txt
  :var0: utunnel
  :var1: tun0

.. cmdinclude:: /_include/interface-description.txt
  :var0: utunnel
  :var1: tun0

.. cmdinclude:: /_include/interface-disable.txt
  :var0: utunnel
  :var1: tun0


.. cfgcmd:: set interfaces utunnel <interface> tunnel-type <tunnel-type>

This is one of the simplest types of custom tunnels, see :ref:`tunnel-type-config`

An example:

.. code-block:: none

  set interfaces utunnel tun0 tunnel-type clash

.. cfgcmd:: set interfaces utunnel <interface> manage-type <external>

This option defines how interface is managed.

* ``external``: Tunnel device is created/deleted by external program.

An example:

.. code-block:: none

  set interfaces utunnel tun0 manage-type external


.. _tunnel-type-config:

Tunnel type configuration
-------------------------

Tunnel-type can be defined by yaml under ``/config/utunnels``.
For example, with ``clash.yaml`` and ``socks5proxy.yaml``, you can do

.. code-block:: none

  set interfaces utunnel tun0 tunnel-type clash
  set interfaces utunnel tun1 tunnel-type socks5proxy

Example file ``/config/utunnels/clash.yaml``:

.. code-block:: yaml

  type: clash
  scripts:
    start: /usr/bin/python3 /config/clash/bin/clashctl.py start {device}
    stop: /usr/bin/python3 /config/clash/bin/clashctl.py stop {device}
    update: /usr/bin/python3 /config/clash/bin/clashctl.py reload {device}
    status: systemctl status clash@{device}



Example project: `vyos-clash`_


.. _`vyos-clash`: https://github.com/sskaje/vyos-clash

