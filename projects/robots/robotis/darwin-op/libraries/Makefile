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

TARGETS=robotis-op2.Makefile managers.Makefile python.Makefile

ifeq ($(OSTYPE),windows)
PYTHON_MAKE = PATH="$(PYTHON27_HOME):$(PATH)" make
else
PYTHON_MAKE = make
endif

.PHONY: release debug profile clean

release debug profile: $(TARGETS)
	cp python27/*managers.* python  # kept for backward compatibility with < R2019b

clean: $(TARGETS)
	# Remove python/*managers.* except python/managers.i
	ls -1 python/*managers.* | grep -v managers.i | xargs rm -f

managers.Makefile: robotis-op2.Makefile

managers.Makefile robotis-op2.Makefile:
	+@echo "# make" $(MAKECMDGOALS) $(@:.Makefile=)
	+@make -s -C $(@:.Makefile=) $(MAKECMDGOALS)

python.Makefile: managers.Makefile
ifeq ($(OSTYPE),linux)
	@echo "# make" $@ python2.7;
	+PYTHON_COMMAND=python2.7 make -s -C python $(MAKECMDGOALS)
ifeq ($(UBUNTU_VERSION), 16.04)
	@echo "# make" $@ python3.5;
	+PYTHON_COMMAND=python3.5 make -s -C python $(MAKECMDGOALS)
endif
	@echo "# make" $@ python3.6;
	+PYTHON_COMMAND=python3.6 make -s -C python $(MAKECMDGOALS)
	@echo "# make" $@ python3.7;
	+PYTHON_COMMAND=python3.7 make -s -C python $(MAKECMDGOALS)
	@echo "# make" $@ python3.8;
	+PYTHON_COMMAND=python3.8 make -s -C python $(MAKECMDGOALS)
endif
ifeq ($(OSTYPE),windows)
	@echo "# make" $@ python3.7;
	+make -s -C python $(MAKECMDGOALS)
	@echo "# make" $@ python2.7;
	+PATH="$(PYTHON27_HOME):$(PATH)" make -s -C python $(MAKECMDGOALS)
endif
ifeq ($(OSTYPE),darwin)
	@echo "# make" $@ python2.7;
	+make -s -C python $(MAKECMDGOALS)
	@echo "# make" $@ python3.7;
	+PYTHON_COMMAND=python3.7 make -s -C python $(MAKECMDGOALS)
endif
