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

	# SRPM oluştur (Sadece base kernel, tools'a dokunmadık, sed komutlarını siktir ettik)
	rpmbuild -bs $$(rpm --eval '%{_specdir}')/kernel.spec \
		--define "_srcrpmdir $(outdir)" \
		--define "fedora 43" \
		--define "dist .fc43" \
		--without debug \
		--without debuginfo \
		--without doc \
		--without perf \
		--without cross_headers \
		--without ipaclones \
		--without efiuki \
		--with baseonly