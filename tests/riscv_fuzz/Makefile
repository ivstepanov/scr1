ARCH := imfc

src_dir := $(CURDIR)
obj_dir   := $(bld_dir)/riscv_fuzz_objs
test_list := riscv_fuzz
objs      := $(addprefix $(obj_dir)/,$(test_list:%=%.o))
test_elf  := $(addprefix $(bld_dir)/,$(test_list:%=%.elf))
test_hex  := $(addprefix $(bld_dir)/,$(test_list:%=%.hex))
test_dump := $(addprefix $(bld_dir)/,$(test_list:%=%.dump))

CFLAGS := -I$(inc_dir) -I$(src_dir) -DASM -Wa,-march=rv32$(ARCH) -march=rv32$(ARCH) -mabi=ilp32f -D__riscv_xlen=32 -g
LDFLAGS := -static -fvisibility=hidden -nostdlib -nostartfiles -T$(inc_dir)/link.ld -march=rv32$(ARCH) -mabi=ilp32f

VPATH += $(src_dir) $(bld_dir) $(obj_dir) $(RISCV_FUZZ_DIR)/work

default: $(test_elf) $(test_hex) $(test_dump)

$(obj_dir) :
	mkdir -p $(obj_dir)

$(obj_dir)/%.o: %.S | $(obj_dir)
	$(RISCV_GCC) -c $< $(CFLAGS) -o $@

$(bld_dir)/%.elf: $(obj_dir)/%.o | $(obj_dir)
	$(RISCV_GCC) $^ $(LDFLAGS) -o $@

$(bld_dir)/%.hex: $(bld_dir)/%.elf
	$(RISCV_OBJCOPY) $^ $@

$(bld_dir)/%.dump: $(bld_dir)/%.elf
	$(RISCV_OBJDUMP) -S $^ > $@

clean:
	$(RM) $(test_elf) $(test_hex) $(test_dump) $(objs)
	$(RM) -R $(obj_dir)


