srpm:
	# Gerekli araçları kur
	dnf install -y rpm-build rpmdevtools git fedpkg sed

	# Çalışma ortamını hazırla
	rpmdev-setuptree

	# Fedora kernel dist-git reposunu klon'la (rc3 commit'i)
	cd /tmp && git clone https://src.fedoraproject.org/rpms/kernel.git fedora-kernel
	cd /tmp/fedora-kernel && git checkout 6a58a88925a0740eda0e53cc40e971a8806d8b90

	# Tarball'ı Fedora lookaside cache'ten indir (spectool bunu yapamaz)
	cd /tmp/fedora-kernel && fedpkg sources

	# dist-git'teki TÜM dosyaları SOURCES dizinine kopyala
	# (scriptler, sertifikalar, yaml, config, patch vb. hepsi)
	find /tmp/fedora-kernel -maxdepth 1 -type f ! -name '*.spec' ! -name 'sources' ! -name '.gitignore' -exec cp {} $$(rpm --eval '%{_sourcedir}')/ \;

	# Spec dosyasını kopyala
	cp /tmp/fedora-kernel/kernel.spec $$(rpm --eval '%{_specdir}')/

	# Bizim config dosyamızı x86_64 config olarak kopyala (üzerine yaz)
	cp $(spec)/Ro-Kernel-Experimental.config $$(rpm --eval '%{_sourcedir}')/kernel-x86_64-fedora.config

	# spec dosyasında buildid'yi ayarla
	sed -i 's/# define buildid .local/%define buildid .Ro_Kernel_Experimental/' $$(rpm --eval '%{_specdir}')/kernel.spec

	# COPR, --without parametrelerini mock build'e aktarmıyor.
	# Bu yüzden spec dosyasına doğrudan müdahale ediyoruz:
	# Sadece base kernel derlensin (debug, perf, tools vb. kapatılıyor)
	sed -i 's/^%define with_debug.*/%define with_debug 0/' $$(rpm --eval '%{_specdir}')/kernel.spec
	sed -i 's/^%define with_debuginfo.*/%define with_debuginfo 0/' $$(rpm --eval '%{_specdir}')/kernel.spec
	sed -i 's/^%define with_perf.*/%define with_perf 0/' $$(rpm --eval '%{_specdir}')/kernel.spec
	sed -i 's/^%define with_tools.*/%define with_tools 0/' $$(rpm --eval '%{_specdir}')/kernel.spec
	sed -i 's/^%define with_bpftool.*/%define with_bpftool 0/' $$(rpm --eval '%{_specdir}')/kernel.spec
	sed -i 's/^%define with_cross_headers.*/%define with_cross_headers 0/' $$(rpm --eval '%{_specdir}')/kernel.spec
	sed -i 's/^%define with_ipaclones.*/%define with_ipaclones 0/' $$(rpm --eval '%{_specdir}')/kernel.spec
	sed -i 's/^%define with_efiuki.*/%define with_efiuki 0/' $$(rpm --eval '%{_specdir}')/kernel.spec
	sed -i 's/^%define with_baseonly.*/%define with_baseonly 1/' $$(rpm --eval '%{_specdir}')/kernel.spec

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
		--without bpftool \
		--without cross_headers \
		--without ipaclones \
		--without efiuki
