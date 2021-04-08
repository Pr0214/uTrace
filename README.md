# uTrace
一个简易的unicorn tracer，剪裁自项目[rainbow](https://github.com/Ledger-Donjon/rainbow)。

使用方法，创建unicorn实例后添加这么一句即可：
**uTrace.UnicornDebugger(mu)**

</p>
example.py
from unicorn import *
from unicorn.arm_const import *
import uTrace

# code to be emulated
THUMB_CODE = b"\x37\x00\xa0\xe3\x03\x10\x42\xe0"  # mov r0, #0x37; sub r1, r2, r3
# memory address where emulation starts
ADDRESS = 0x10000

# Initialize emulator in thumb mode
mu = Uc(UC_ARCH_ARM, UC_MODE_THUMB)

# 附加uTrace调试器
uTrace.UnicornDebugger(mu)

# map 2MB memory for this emulation
mu.mem_map(ADDRESS, 2 * 1024 * 1024)
# write machine code to be emulated to memory
mu.mem_write(ADDRESS, THUMB_CODE)
# initialize reg
mu.reg_write(UC_ARM_REG_R0,0x1)
mu.reg_write(UC_ARM_REG_R0,0x2)
mu.reg_write(UC_ARM_REG_R0,0x3)

mu.emu_start(ADDRESS+1, until=0, count=2)

</p>

rainbow是一个小巧而美的unicorn trace项目，其中的trace打印效果很漂亮，本项目剪裁了其中arm32 trace部分并稍作完善，适合以下场景

* 逆向算法时trace 进行指令还原
* 开发自己的unicorn项目时，使用uTrace debug

目前还很简陋，可以和androidnativeemu配合使用，接下来希望编写对应的uEmulator。



