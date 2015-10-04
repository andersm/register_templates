# register_templates
C++11 templates for register access, based on [Ken Smith's talk at CppCon 2015](https://github.com/kensmith/cppmmio).

Usage example:

	using UsbRxf = reg_t<ro_t, uint8_t, 0x22200001, 1, 0>; // UsbRxf is bit 0 of the 8-bit read-only register located at address 0x22200001
	using UsbTxe = reg_t<ro_t, uint8_t, 0x22200001, 1, 1>; // UsbTxe is bit 1 of the same register
	using UsbFifo = reg_t<rw_t, uint8_t, 0x22100001, 8, 0>; // UsbFifo is an 8-bit read-write register located at address 0x22100001
	
	uint8_t ReceiveByte(void)
	{
		while (UsbRxf::read())
			;
		return UsbFifo::read();
	}
	
	void SendByte(uint8_t byte)
	{
		while (UsbTxe::read())
			;
		UsbFifo::write(byte);
	}
