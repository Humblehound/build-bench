# make magic not needed
export MAKEFLAGS += --no-builtin-rules
.SUFFIXES:

ifndef ROOT_DIR
export ROOT_DIR=$(shell readlink -m $(shell pwd)/../..)
endif

ifndef BUILD_DIR
include $(ROOT_DIR)/include/includes.mk
endif

include $(ROOT_DIR)/include/generators.mk

.PHONY: default
default: bazel

include $(ROOT_DIR)/include/time.mk

export BAZEL_DIR=$(CACHE_DIR)/bazel

export BAZEL_OPTIONS:=--test_verbose_timeout_warnings --verbose_failures --legacy_bazel_java_test --javacopt='-extra_checks:off' --genrule_strategy=standalone --spawn_strategy=standalone --curses=no

.PHONY: bazel
bazel: bazel$(BAZEL_DEFAULT_VERSION)

.PHONY: version
version: $(BAZEL_DIR)/bazel$(BAZEL_DEFAULT_VERSION)/bin/bazel
	$(BAZEL_DIR)/bazel$(BAZEL_DEFAULT_VERSION)/bin/bazel version

.NOTPARALLEL: bazel%
.PHONY: bazel%
bazel%: $(CONFIGURED_BUILD_ROOT)/bazel%/BUILD $(BAZEL_DIR)/bazel%/bin/bazel $(REPORTS_DIR)/$(TARGET_NAME).csv
	$(info ******* bazel start)
# fetch command has no spawn_strategy option, required on Travis CI
	cd $(CONFIGURED_BUILD_ROOT)/bazel$*; $(BAZEL_DIR)/bazel$*/bin/bazel --nomaster_bazelrc --bazelrc=$(BAZEL_DIR)/bazel$*/bazel.bazelrc --output_base=$(CONFIGURED_BUILD_ROOT)/bazel$*/bazel_cache fetch -- :all
	cd $(CONFIGURED_BUILD_ROOT)/bazel$*; $(call TIME_CMD,$@) $(BAZEL_DIR)/bazel$*/bin/bazel --nomaster_bazelrc --bazelrc=$(BAZEL_DIR)/bazel$*/bazel.bazelrc --output_base=$(CONFIGURED_BUILD_ROOT)/bazel$*/bazel_cache test $(BAZEL_COMMAND_OPTIONS) $(BAZEL_OPTIONS) //:example_tests

.PRECIOUS: $(BAZEL_DIR)/bazel-%-installer-linux-x86_64.sh
$(BAZEL_DIR)/bazel-%-installer-linux-x86_64.sh:
	@mkdir -p $(BAZEL_DIR)
	@cd $(BAZEL_DIR); wget --quiet https://github.com/bazelbuild/bazel/releases/download/$*/bazel-$*-installer-linux-x86_64.sh
	@cd $(BAZEL_DIR); chmod u+x bazel-$*-installer-linux-x86_64.sh

.PRECIOUS: $(BAZEL_DIR)/bazel%/bazel.bazelrc
$(BAZEL_DIR)/bazel%/bazel.bazelrc:
	mkdir -p $(BAZEL_DIR)/bazel$*
	echo "startup --watchfs" > $(BAZEL_DIR)/bazel$*/bazel.bazelrc

.PRECIOUS: $(BAZEL_DIR)/bazel%/bin/bazel
$(BAZEL_DIR)/bazel%/bin/bazel: $(BAZEL_DIR)/bazel-%-installer-linux-x86_64.sh $(BAZEL_DIR)/bazel%/bazel.bazelrc
	@mkdir -p $(BAZEL_DIR)
	cd $(BAZEL_DIR); ./bazel-$*-installer-linux-x86_64.sh --prefix=$(BAZEL_DIR)/bazel$* --bazelrc=$(BAZEL_DIR)/bazel$*/bazel.bazelrc
# touch so that make remembers next time
	@touch $(BAZEL_DIR)/bazel$*/bin/bazel

.PRECIOUS: $(CONFIGURED_BUILD_ROOT)/bazel%/BUILD
$(CONFIGURED_BUILD_ROOT)/bazel%/BUILD: $(CONFIGURED_BUILD_SOURCE)
	@python $(SCRIPTS_DIR)/apply-templates.py $(BUILDTEMPLATES_DIR)/$(BUILD_DEFINITIONS)/bazel $(CONFIGURED_BUILD_ROOT)/bazel$* --subprojectnum=$(SUBPROJECT_NUM) --filenum=$(FILE_NUM)
