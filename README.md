# PyEW

A Python interface to the Earthworm Seismic Data System

## Installation

In order to install PyEW:

### Dependencies

We suggest the usage of Anaconda for deployment:

    $ conda create --name PyEW
    $ conda activate PyEW

### Procedure

These commands take place in the PyEW directory:

    $ cd src/
    $ python setup.py build_ext -i

A file named PyEW.cpython-36m-x86_64-linux-gnu.so should have been created.
We can test this works by importing into python:

    $ python
    >>> import PyEW

## Usage:
PyEW has various classes that have various degrees of abstractions. For example:

#### PyEW.ring:
  * **PyEW.ring(ring_id):**  
  This will initiate a ring object that can communicate with ring: ring_id.
  
  * **PyEW.ring().attach():**  
  This method will attach the ring.
  
  * **PyEW.ring().detach():**  
  This method will detach the ring.
  
  * **PyEW.ring().get_buffer():**  
  This method will return the pointer to the memory buffer.
  
#### PyEW.transport:
  * **PyEW.transport(ring_id, mod, inst):**  
  This will initiate a transport object with ring: ring_id, module id: mod, and installation id: inst. This object will be automatically attached to the ring.
  
  * **PyEW.transport().detach():**  
  This method detaches the object from the ring. No way to re-attach.
  
  * **PyEW.transport().putmsg(mtype, msg, size):**  
  This method will insert a message into a ring of type: mtype, bytestring: msg, and size: size.
  
  * **PyEW.transport().getmsg_type(mtype):**  
  This method will attempt to get a message from the ring. Returns a tuple of msg.lengh and msg. The tuple will be 0, 0 if unsuccesfull.
  
  * **PyEW.transport().copymsg_type(mtype)**  
  This method will attempt to copy the messagea from the ring. Returns a tuple of msg.lengh and msg. The tuple will be 0, 0 if unsuccesfull.
  
  * **PyEW.transport().reqsta()**  
  This method will return a tuple with the system status.
  
### Yet the highest abstraction layer is:
#### PyEW.EWModule:
  * **PyEW.EWModule(def_ring, mod_id, inst_id, hb_time, db):**  
  This will initiate a EW Module object with a default ring: def_ring, module id: mod_id, installation id: inst_id, heartbeat interval: hb_time, and debuging set to FALSE (by default). This module will initiate a heartbeat thread that will start by default, it will initate a listener by default to stop if EW emitts a stop message. These listeners and heartbeats will be sent to the default ring.
  
  * **PyEW.EWModule().goodbye():**  
  This method will begin the mod shutdown if called from somewhere else. It is important to shut down all listener threads before any attempt at shutdown of a program. It will print 'Gracefull Shutdown' when ready to end.
  
  * **PyEW.EWModule().mod_sta():**  
  This method will return a boolean value that reflects if the internal state is ok. Listen to this value to check if there has been a request for the module shutdown.
  
  * **PyEW.EWModule().req_syssta()**  
  This method will request and print the EW System status. Mainly used for testing purposes.
  
  * **PyEW.EWModule().add_ring(ring_id):**  
  This will add a ring to an internal buffer of rings. The ring id: ring_id given will add the listener. For example: Mod.add_ring(1000) will add ring **1000** at location **0**. Should you call this method again (e.g. Mod.add_ring(1005)) will add ring **1005** at location **1** and so on. You can add many rings for multiple inputs and outputs.
  
  * **PyEW.EWModule().get_bytes(buf_ring, msg_type)  
  PyEW.EWModule().get_msg(buf_ring, msg_type):**
  
  * **PyEW.EWModule().put_bytes(buf_ring, msg_type, msg)  
  PyEW.EWModule().put_msg(buf_ring, msg_type, msg):**
  
  * PyEW.EWModule().get_wave(buf_ring)
  
  * PyEW.EWModule().put_wave(buf_ring, msg)

