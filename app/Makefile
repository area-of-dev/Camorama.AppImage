# Copyright 2020 Alex Woroschilow (alex.woroschilow@gmail.com)
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
PWD:=$(shell pwd)

.PHONY: all

all: clean init
	mkdir --parents $(PWD)/build/Boilerplate.AppDir

	wget --output-document=$(PWD)/build/build.zip https://github.com/alessio/camorama/archive/refs/heads/master.zip
	unzip $(PWD)/build/build.zip -d $(PWD)/build
	cd $(PWD)/build/camorama-master && ./autogen.sh --prefix=$(PWD)/build/tmp
	cd $(PWD)/build/camorama-master && sed -i -e 's#$(PWD)/build/tmp#./..#g' ./config.h

	cd $(PWD)/build/camorama-master && make
	cd $(PWD)/build/camorama-master && make install

	cp --recursive --force $(PWD)/build/tmp/* \
		$(PWD)/build/Boilerplate.AppDir

	$(PWD)/bin/Apprepo.AppImage --destination=$(PWD)/build appdir boilerplate \
	    libgdk-pixbuf2.0-0 libatk1.0-0 libatk-bridge2.0-0 libgtk2.0-0 \
	    libreadline8 at-spi2-core libpango-1.0-0 libgtk-3-0 \
		libwayland-cursor0 libselinux1 

	echo ''                                   >> $(PWD)/build/Boilerplate.AppDir/AppRun
	echo 'cd $${APPDIR}/bin'                  >> $(PWD)/build/Boilerplate.AppDir/AppRun
	echo './camorama "$${@}"'                 >> $(PWD)/build/Boilerplate.AppDir/AppRun

	rm --force $(PWD)/build/Boilerplate.AppDir/*.desktop
	cp --force $(PWD)/AppDir/*.desktop  $(PWD)/build/Boilerplate.AppDir/ || true
	cp --force $(PWD)/AppDir/*.png      $(PWD)/build/Boilerplate.AppDir/ || true

	export ARCH=x86_64 && $(PWD)/bin/appimagetool.AppImage $(PWD)/build/Boilerplate.AppDir $(PWD)/Camorama.AppImage
	chmod +x $(PWD)/Camorama.AppImage

init:
	mkdir --parents $(PWD)/bin

	wget https://apprepo.de/appimage/download/apprepo \
		--output-document=$(PWD)/bin/Apprepo.AppImage
	chmod +x $(PWD)/bin/Apprepo.AppImage

	wget https://github.com/AppImage/AppImageKit/releases/download/13/appimagetool-x86_64.AppImage \
		--output-document=$(PWD)/bin/appimagetool.AppImage
	chmod +x $(PWD)/bin/appimagetool.AppImage

clean:
	rm -rf $(PWD)/*.AppImage
	rm -rf $(PWD)/bin/*
	rm -rf $(PWD)/build
