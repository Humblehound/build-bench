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
default: fsbt

include $(ROOT_DIR)/include/time.mk

export FSBT_DIR=$(CACHE_DIR)/fsbt

.PHONY: fsbt
fsbt: fsbt$(FSBT_DEFAULT_VERSION)


#.PHONY: version
#version: $(FSBT_DIR)/fsbt$(FSBT_DEFAULT_VERSION)/bin/fsbt
#	$(FSBT_DIR)/fsbt$(FSBT_DEFAULT_VERSION)/bin/sbt about


.NOTPARALLEL: fsbt%
.PHONY: fsbt%
fsbt%:
	$(info ******* fsbt start)
	cd $(CONFIGURED_BUILD_ROOT)/fsbt$*; $(call TIME_CMD,$@) fsbt test package
#$(CONFIGURED_BUILD_ROOT)/sbt%/build.sbt $(SBT_DIR)/sbt%/bin/sbt $(TIME) $(REPORTS_DIR)
#	cd $(CONFIGURED_BUILD_ROOT)/sbt$*; $(SBT_DIR)/$@/bin/sbt -java-home $(JAVA_HOME) about


#.NOTPARALLEL: sbt%
#.PHONY: sbt%
#sbt%: $(CONFIGURED_BUILD_ROOT)/sbt%/build.sbt $(SBT_DIR)/sbt%/bin/sbt $(TIME) $(REPORTS_DIR)
#	cd $(CONFIGURED_BUILD_ROOT)/sbt$*; $(SBT_DIR)/$@/bin/sbt -java-home $(JAVA_HOME) about
#	$(info ******* sbt start)
#	cd $(CONFIGURED_BUILD_ROOT)/sbt$*; $(call TIME_CMD,$@) $(SBT_DIR)/$@/bin/sbt -java-home $(JAVA_HOME) test package



#.PRECIOUS: $(SBT_DIR)/sbt-%.tgz
#$(SBT_DIR)/sbt-%.tgz:
#	@mkdir -p $(SBT_DIR)
#	@cd $(SBT_DIR); wget --quiet https://github.com/sbt/sbt/releases/download/v$*/sbt-$*.tgz
#
#
#.PRECIOUS: $(SBT_DIR)/sbt%/bin/sbt
#$(SBT_DIR)/sbt%/bin/sbt: $(SBT_DIR)/sbt-%.tgz
#	@mkdir -p $(SBT_DIR)/sbt$*
#	@cd $(SBT_DIR);tar -xzf sbt-$*.tgz -C sbt$* --strip-components 1
#
#.PRECIOUS: $(CONFIGURED_BUILD_ROOT)/sbt%/build.sbt
#$(CONFIGURED_BUILD_ROOT)/sbt%/build.sbt: $(CONFIGURED_BUILD_SOURCE)
#	@python $(SCRIPTS_DIR)/apply-templates.py $(BUILDTEMPLATES_DIR)/$(BUILD_DEFINITIONS)/sbt $(CONFIGURED_BUILD_ROOT)/sbt$* --subprojectnum=$(SUBPROJECT_NUM) --filenum=$(FILE_NUM)

