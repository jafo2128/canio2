canio2 project is C++/Python libs to do different I/O operations in 
CAN/CANopen/CANopen-like (e.g. PMD Magellan controller) networks(devices).

canio2 C++ lib is header only.
canio2 python 2 binding is done via boost.python.

Binaries were successfully built on Windows and QNX (Python version - 2.7).

Tips:
 - canio2 lib has a plugins system to use different CAN interfaces (e.g. IXXAT 
USB-to-CAN). You can add support of your CAN interface, see can_plugin.hpp and
ixxat plugin example.
 - to run python script place canio2.pyd and plugin dll (e.g. plg_ixxat.dll) into
the script folder
 - async CAN events can be handled via triggers/waiters subsystem 

##############################
#--- Python script example --#
##############################
import canio2
from canio2 import CANopenNode

module = canio2.make_module('ixxat') 
iface = module.make_iface('HW104122','1000K')
io_service = canio2.IOService(iface)

NODE_ID = 1
node = CANopenNode(NODE_ID, io_service) 
if node.reset(5000) == False:
    print 'Error: Boot Timeout'
else:
    print 'Boot OK'
print 'Node State:', node.state

device_type= node.ru32(0x1000,0)
print 'Device Type:',hex(device_type)