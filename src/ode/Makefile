# Copyright 1996-2019 Cyberbotics Ltd.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

space :=
space +=
WEBOTS_HOME_PATH=$(subst $(space),\ ,$(strip $(subst \,/,$(WEBOTS_HOME))))
include $(WEBOTS_HOME_PATH)/resources/Makefile.os.include

ifeq ($(OSTYPE),windows)
TARGET_ODE_LIBRARY=../../msys64/mingw64/bin/ode.dll
endif
ifeq ($(OSTYPE),linux)
TARGET_ODE_LIBRARY=../../lib/libode.so
endif
ifeq ($(OSTYPE),darwin)
TARGET_ODE_LIBRARY=../../lib/libode.dylib
endif

ifeq ($(MAKECMDGOALS),)
default:
	@echo "Type one of the following:"
	@echo "  make debug                 for debug (with gdb symbols)"
	@echo "  make profile               for profiling (with gprof information)"
	@echo "  make release               for release"
	@echo "  make install               copy the library in the required folder"
	@echo "  make clean                 remove build directory"
else ifeq ($(MAKECMDGOALS),install)

$(TARGET_ODE_LIBRARY):
	@echo "#" copying $(@F) to $@
	@cp $(@F) $@
ifeq ($(OSTYPE),darwin)
	@install_name_tool -id @rpath/lib/libode.dylib $@
endif

install: $(TARGET_ODE_LIBRARY)

else

ifeq ($(MAKECMDGOALS),profile)
  STATIC_LIBRARY = true
endif

FILES_TO_REMOVE = $(TARGET_ODE_LIBRARY) libode.a ode.a
WEBOTS_DISABLE_BINARY_COPY=true

CFLAGS = -DdTRIMESH_ENABLED -DdTRIMESH_OPCODE -DODE_MT -DODE_LIB -DODE_THREADMODE_PTHREAD
ifneq ($(OSTYPE),darwin)
  CFLAGS += -Wno-misleading-indentation -Wno-nonnull-compare
endif
CXXFLAGS = -std=c++11
INCLUDE = -I../../include/ode -I../../include/ode/ode/fluid_dynamics -Iode/src -Iode/src/fluid_dynamics -Ilibccd/src -IOPCODE

# define the source files
C_SOURCES = $(wildcard libccd/src/*.c) $(wildcard ode/src/*.c)
CXX_SOURCES = $(wildcard ode/src/*.cpp) $(wildcard ode/src/joints/*.cpp) $(wildcard ode/src/ode_MT/*.cpp) $(wildcard ode/src/ode_MT/*/*.cpp) $(wildcard OPCODE/*.cpp) $(wildcard OPCODE/Ice/*.cpp) $(wildcard ode/src/fluid_dynamics/*.cpp) $(wildcard ode/src/fluid_dynamics/immersion_link/*.cpp)

# finally, include the Makefile.include to perform magic
EXCLUDE_CONTROLLERS = true
include $(WEBOTS_HOME_PATH)/resources/Makefile.include
endif
