srpm:
	# Gerekli araçları kur
	dnf install -y rpm-build rpmdevtools git fedpkg sed

	# Çalışma ortamını hazırla
	rpmdev-setuptree

	# Fedora kernel dist-git reposunu klon'la (rc3 commit'i)
	cd /tmp && git clone https://src.fedoraproject.org/rpms/kernel.git fedora-kernel
	cd /tmp/fedora-kernel && git checkout 6a58a88925a0740eda0e53cc40e971a8806d8b90

	# Kaynak dosyaları (tarball vb.) indir
	cd /tmp/fedora-kernel && spectool -g -C $$(rpm --eval '%{_sourcedir}') kernel.spec

	# Spec ve patches dosyalarını SOURCES dizinine kopyala
	cp /tmp/fedora-kernel/*.config $$(rpm --eval '%{_sourcedir}')/
	cp /tmp/fedora-kernel/*.patch $$(rpm --eval '%{_sourcedir}')/ 2>/dev/null || true
	cp /tmp/fedora-kernel/kernel.spec $$(rpm --eval '%{_specdir}')/

	# Bizim config dosyamızı x86_64 config olarak kopyala (üzerine yaz)
	cp $(spec)/Ro-Kernel-Experimental.config $$(rpm --eval '%{_sourcedir}')/kernel-x86_64-fedora.config

	# spec dosyasında buildid'yi ayarla
	sed -i 's/# define buildid .local/%define buildid .Ro_Kernel_Experimental/' $$(rpm --eval '%{_specdir}')/kernel.spec

	# SRPM oluştur (sadece base kernel, debug ve extras olmadan)
	rpmbuild -bs $$(rpm --eval '%{_specdir}')/kernel.spec \
		--define "_srcrpmdir $(outdir)" \
		--define "fedora 43" \
		--define "dist .fc43" \
		--without debug \
		--without debuginfo \
		--with baseonly \
		--without doc \
		--without perf \
		--without tools \
		--without bpftool
