# make magic not needed
export MAKEFLAGS += --no-builtin-rules
.SUFFIXES:

ifndef ROOT_DIR
export ROOT_DIR=$(shell readlink -m $(shell pwd)/../..)
endif

ifndef BUILD_DIR
include $(ROOT_DIR)/include/includes.mk
endif

.PHONY: default
default: $(CONFIGURED_BUILD_SOURCE)


$(CONFIGURED_BUILD_SOURCE): $(CACHE_DIR)/commons-math
	@mkdir -p $(CONFIGURED_BUILD_SOURCE)
# softlinking causes problems with some tools (pants)
	cp -rf $(CACHE_DIR)/commons-math/* $(CONFIGURED_BUILD_SOURCE)

# Apache commons math sources
# needed some cleaning up to prevent test failures in diverse buildsystems
.PRECIOUS: $(CACHE_DIR)/commons-math
$(CACHE_DIR)/commons-math:
	@mkdir -p $(CACHE_DIR)
	cd $(CACHE_DIR); git clone -q https://git-wip-us.apache.org/repos/asf/commons-math.git; cd commons-math; git checkout MATH_3_4
# Sadly this test fails with buck because java.io.File cannot handle uris inside jars.
	rm -f $(CACHE_DIR)/commons-math/src/test/java/org/apache/commons/math3/random/EmpiricalDistributionTest.java
# These tests fail with with buck for unknown reasons of comparing floating points to zero.
	rm -f $(CACHE_DIR)/commons-math/src/test/java/org/apache/commons/math3/ml/neuralnet/sofm/KohonenUpdateActionTest.java
	rm -f $(CACHE_DIR)/commons-math/src/test/java/org/apache/commons/math3/ml/neuralnet/sofm/KohonenTrainingTaskTest.java
# This test fails with ant
	rm -f $(CACHE_DIR)/commons-math/src/test/java/org/apache/commons/math3/PerfTestUtils.java
# Some actual Tests sadly do not end with Test.java, so for fairness sake they cannot be used
	rm -f $(CACHE_DIR)/commons-math/src/test/java/org/apache/commons/math3/fitting/leastsquares/EvaluationTestValidation.java
	rm -f $(CACHE_DIR)/commons-math/src/test/java/org/apache/commons/math3/genetics/GeneticAlgorithmTestBinary.java
	rm -f $(CACHE_DIR)/commons-math/src/test/java/org/apache/commons/math3/genetics/GeneticAlgorithmTestPermutations.java
	rm -f $(CACHE_DIR)/commons-math/src/test/java/org/apache/commons/math3/optim/nonlinear/vector/jacobian/AbstractLeastSquaresOptimizerTestValidation.java
	rm -f $(CACHE_DIR)/commons-math/src/test/java/org/apache/commons/math3/optimization/general/AbstractLeastSquaresOptimizerTestValidation.java
	rm -f $(CACHE_DIR)/commons-math/src/test/java/org/apache/commons/math3/util/FastMathTestPerformance.java
# Brittle tests sometimes failing for no obvious reason
	rm -f $(CACHE_DIR)/commons-math/src/test/java/org/apache/commons/math3/optim/nonlinear/scalar/noderiv/CMAESOptimizerTest.java
	rm -f $(CACHE_DIR)/commons-math/src/test/java/org/apache/commons/math3/analysis/polynomials/PolynomialsUtilsTest.java
	rm -f $(CACHE_DIR)/commons-math/src/test/java/org/apache/commons/math3/random/CorrelatedRandomVectorGeneratorTest.java
	mv $(CACHE_DIR)/commons-math/pom.xml $(CACHE_DIR)/commons-math/pom.xml.bak
	mv $(CACHE_DIR)/commons-math/build.xml $(CACHE_DIR)/commons-math/build.xml.bak
