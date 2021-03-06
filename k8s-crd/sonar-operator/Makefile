# Copyright 2016-2017 The OpenEBS Authors.
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

.PHONY: image clean bootstrap deps build image
.DEFAULT_GOAL := build

# Tools required for different make targets or for development purposes
EXTERNAL_TOOLS=\
	github.com/Masterminds/glide 

# Bootstrap the build by downloading additional tools
bootstrap:
	@for tool in  $(EXTERNAL_TOOLS) ; do \
		echo "Installing $$tool" ; \
		go get $$tool; \
	done 

deps: 
	rm -rf vendor
	@glide up
	glide install -v

build: $(shell find . -name "*.go")
	CGO_ENABLED=0 go build -a -ldflags '-extldflags "-static"' -o sonar-operator .

image: 
	@cp sonar-operator buildscripts/docker/
	@cd buildscripts/docker && sudo docker build -t openebs/sonar-operator:ci .
	@sh buildscripts/push

clean:
	rm sonar-operator
