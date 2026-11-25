.PHONY: install-chartsnap snapshot update-snapshots
install-chartsnap:
	helm plugin install https://github.com/jlandowner/helm-chartsnap

snapshot:
	helm chartsnap -c ../ -f .

update-snapshots:
	helm chartsnap -c ../ -f . -u
