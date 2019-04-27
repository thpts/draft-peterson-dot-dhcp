.PHONY: clean generate

venv: requirements.txt
	type virtualenv >/dev/null
	rm -rf venv
	(virtualenv venv --python=`which python3.6` && venv/bin/pip install --upgrade pip && venv/bin/pip install -r $<) || rm -rf venv

clean:
	rm -rf venv

generate: venv
	PYTHONPATH=src:${PYTHONPATH} protocol "Code:8,Len:8,DNS Servers:32,...:48" > dhcpv4.txt
	PYTHONPATH=src:${PYTHONPATH} protocol "option-code:16,option-len:16,DNS Server:128,...:32" > dhcpv6.txt
	PYTHONPATH=src:${PYTHONPATH} protocol "Type:8,Len:8,DNS Servers:128,...:16" > ra.txt
	cat {dhcpv4,dhcpv6,ra}.txt
