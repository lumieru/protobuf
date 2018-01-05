
ANDROID_BUILD_DIR = _android_build
IOS_BUILD_DIR = _ios_build
CURR_DIR = $(shell pwd)
LIBPROTOBUF_LITE = libprotobuf-lite

.PHONY: help clean android ios ioscopyheader

help:
	@echo "use \`make <target> where <target> is:\`"
	@echo "    help: display this help"
	@echo "    clean: clean the build directory"
	@echo "    android: build libprotobuf-lite for android"
	@echo "    ios: build libprotobuf-lite for ios"

clean:
	if [ -d $(ANDROID_BUILD_DIR) ]; then \
		rm -r $(ANDROID_BUILD_DIR); \
	fi
	if [ -d $(IOS_BUILD_DIR) ]; then \
		rm -r $(IOS_BUILD_DIR); \
	fi

android:
	@echo "Not implemented yet"

ios: ioscopyheader
	mkdir -p $(IOS_BUILD_DIR); \
	cd $(IOS_BUILD_DIR); \
	plats=('OS' 'SIMULATOR' 'SIMULATOR64'); \
	for pl in $${plats[@]}; do \
		mkdir -p $$pl; \
		cd $$pl; \
		installDir=$(CURR_DIR)/$(IOS_BUILD_DIR)/install/$$pl; \
		cmake ../../../cmake \
			-DCMAKE_TOOLCHAIN_FILE=../../ios.toolchain.cmake \
			-DCMAKE_INSTALL_PREFIX=$$installDir \
			-Dprotobuf_BUILD_TESTS=false \
			-Dprotobuf_BUILD_PROTOC=false \
			-Dprotobuf_BUILD_LIBPROTOC=false \
			-DENABLE_BITCODE=false \
			-DIOS_DEPLOYMENT_TARGET=7.0 \
			-DIOS_PLATFORM=$$pl; \
		make libprotobuf-lite; \
		cd ..; \
	done; \
	mkdir -p $(LIBPROTOBUF_LITE); \
	lipo -create \
		-output $(LIBPROTOBUF_LITE)/$(LIBPROTOBUF_LITE).a \
		OS/$(LIBPROTOBUF_LITE).a \
		SIMULATOR/$(LIBPROTOBUF_LITE).a \
		SIMULATOR64/$(LIBPROTOBUF_LITE).a;

ioscopyheader:
	mkdir -p $(IOS_BUILD_DIR); \
	cd $(IOS_BUILD_DIR); \
	mkdir -p $(LIBPROTOBUF)/include; \
	mkdir -p $(LIBPROTOBUF_LITE)/include; \
	liteInc=$(LIBPROTOBUF_LITE)/include; \
	cd $$liteInc; \
	rm -r *; \
	lite_heads=( \
		google/protobuf/arena.h \
		google/protobuf/arena_impl.h \
		google/protobuf/arenastring.h \
		google/protobuf/extension_set.h \
		google/protobuf/generated_message_util.h \
		google/protobuf/io/coded_stream.h \
		google/protobuf/io/zero_copy_stream.h \
		google/protobuf/io/zero_copy_stream_impl_lite.h \
		google/protobuf/message_lite.h \
		google/protobuf/repeated_field.h \
		google/protobuf/stubs/bytestream.h \
		google/protobuf/stubs/common.h \
		google/protobuf/stubs/int128.h \
		google/protobuf/stubs/once.h \
		google/protobuf/stubs/status.h \
		google/protobuf/stubs/statusor.h \
		google/protobuf/stubs/stringpiece.h \
		google/protobuf/stubs/stringprintf.h \
		google/protobuf/stubs/strutil.h \
		google/protobuf/stubs/time.h \
		google/protobuf/stubs/port.h \
		google/protobuf/stubs/platform_macros.h \
		google/protobuf/stubs/macros.h \
		google/protobuf/stubs/logging.h \
		google/protobuf/stubs/scoped_ptr.h \
		google/protobuf/stubs/mutex.h \
		google/protobuf/stubs/callback.h \
		google/protobuf/stubs/type_traits.h \
		google/protobuf/stubs/template_util.h \
		google/protobuf/stubs/fastmem.h \
		google/protobuf/stubs/casts.h \
		google/protobuf/stubs/shared_ptr.h \
		google/protobuf/stubs/stl_util.h \
		google/protobuf/stubs/hash.h \
		google/protobuf/stubs/atomicops.h \
		google/protobuf/stubs/atomic_sequence_num.h \
		google/protobuf/stubs/atomicops_internals_generic_gcc.h \
		google/protobuf/stubs/atomicops_internals_x86_msvc.h \
		google/protobuf/stubs/atomicops_internals_arm64_gcc.h \
		google/protobuf/stubs/atomicops_internals_x86_gcc.h \
		google/protobuf/stubs/atomicops_internals_tsan.h \
		google/protobuf/stubs/atomicops_internals_solaris.h \
		google/protobuf/stubs/atomicops_internals_power.h \
		google/protobuf/stubs/atomicops_internals_arm_gcc.h \
		google/protobuf/stubs/atomicops_internals_arm_qnx.h \
		google/protobuf/stubs/atomicops_internals_mips_gcc.h \
		google/protobuf/stubs/atomicops_internals_power.h \
		google/protobuf/stubs/atomicops_internals_generic_c11_atomic.h \
		google/protobuf/stubs/atomicops_internals_ppc_gcc.h \
		google/protobuf/generated_message_table_driven.h \
		google/protobuf/has_bits.h \
		google/protobuf/metadata_lite.h \
		google/protobuf/generated_enum_util.h \
		google/protobuf/wire_format_lite.h \
		google/protobuf/wire_format_lite_inl.h \
		google/protobuf/map.h \
		google/protobuf/map_entry_lite.h \
		google/protobuf/map_type_handler.h \
		google/protobuf/map_field_lite.h \
	); \
	for lh in $${lite_heads[@]}; do \
		mkdir -p $$(dirname $$lh);\
		cp $(CURR_DIR)/../src/$$lh $$lh; \
	done;


