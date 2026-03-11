srpm:
	# Gerekli araçları kur
	dnf install -y rpm-build koji rpmdevtools sed

	# Çalışma ortamını hazırla
	rpmdev-setuptree

	# Fedora rawhide kernel SRPM'ini Koji'den indir
	cd /tmp && koji download-build --arch=src kernel-7.0.0-0.rc3.28.fc45

	# SRPM'i aç
	rpm -ivh /tmp/kernel-*.src.rpm

	# Bizim config dosyamızı x86_64 config olarak kopyala
	cp $(spec_srcdir)/Ro-Kernel-Experimental.config $$(rpm --eval '%{_sourcedir}')/kernel-x86_64-fedora.config

	# spec dosyasında buildid'yi ayarla
	sed -i 's/# define buildid .local/%define buildid .Ro_Kernel_Experimental/' $$(rpm --eval '%{_specdir}')/kernel.spec

	# SRPM oluştur (sadece base kernel, debug ve doc olmadan)
	rpmbuild -bs $$(rpm --eval '%{_specdir}')/kernel.spec \
		--define "_srcrpmdir $(outdir)" \
		--without debug \
		--without debuginfo \
		--with baseonly \
		--without doc \
		--without perf \
		--without tools \
		--without bpftool
