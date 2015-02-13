#
# Makefile to generate docid.exe
#
# Attention: La commande $(shell [ -f <filename>]) ne fonctionne pas
#
# Author: Olivier Appere
# Date: 07th of July 2014
#
ifndef VERSION
VERSION=3_6_2
endif
	
DIST = dist
GUI_EXE = docid.exe
GUI_BACKUP_EXE = docid_backup.exe
CLI_EXE = docid_cli.exe
MARKDOWN = lib/markdown2.py

#
# Configuration:
#
PYTHON = python
MAKENSIS = makensis.exe
ZIP2EXE = Contrib\zip2exe
OUTPUT = 
WEBSERVER = C:\xampplite\htdocs\qams\docid
#WORKAREA = C:\Documents\ and\ Settings\appereo1\Mes%20documents\Synergy\ccm_wa\db_sms_pds\TOOLS_QA-dev_appere\TOOLS_QA\doCID
#WORKAREA = C:\DOCUME~1\appereo1\Mes*\Synergy\ccm_wa\db_sms_pds\TOOLS_QA-dev_appere\TOOLS_QA\doCID
WORKAREA = C:\synergy_workarea\db_sms_pds\TOOLS_QA-dev_appere\TOOLS_QA\doCID
#
# The project to be built
#
default: nsis copy_docs copy copy_wrk_area

gui:
	@echo ���������������������������������������������������������������������ͻ
	@echo �            doCID windows mode executable generation ...             �
	@echo ���������������������������������������������������������������������ͼ
	@$(PYTHON) setup.py py2exe
	@rm -f -r -v $(DIST)/result/*.*
	@touch $(DIST)/result/empty.txt
	@rm -f -r -v $(DIST)/actions/*.*
	@touch $(DIST)/actions/actions.txt	
	@cp conf/docid_empty.ini $(DIST)/conf/docid.ini
	
cli:
	@echo ���������������������������������������������������������������������ͻ
	@echo �            doCID console mode executable generation ...             �
	@echo ���������������������������������������������������������������������ͼ
	@mv $(DIST)/$(GUI_EXE) $(DIST)/$(GUI_BACKUP_EXE)
	@$(PYTHON) setup_dos.py py2exe
	@mv $(DIST)/$(GUI_EXE) $(DIST)/$(CLI_EXE)
	@mv $(DIST)/$(GUI_BACKUP_EXE) $(DIST)/$(GUI_EXE)
	
docs:
	@echo ���������������������������������������������������������������������ͻ
	@echo �            doCID documentation generation ...                       �
	@echo ���������������������������������������������������������������������ͼ
	cd doc && make.bat html

copy_docs: docs
	@echo ���������������������������������������������������������������������ͻ
	@echo �            doCID documentation copy on web server ...               �
	@echo ���������������������������������������������������������������������ͼ
	xcopy doc\_build\html $(WEBSERVER) /e /y
	
# $(MAKE) -f Makefile html -C doc
	
nsis: gui docs
	@echo ���������������������������������������������������������������������ͻ
	@echo �            doCID installer generation ...                           �
	@echo ���������������������������������������������������������������������ͼ
	@$(PATHNSIS)$(MAKENSIS) create_install.nsi
	
copy:
	@echo ���������������������������������������������������������������������ͻ
	@echo �            Copy doCID binary on webserver ...                       �
	@echo ���������������������������������������������������������������������ͼ
	@mv docid_installer.exe doCID_v$(VERSION)_install.exe
	@cp doCID_v$(VERSION)_install.exe $(WEBSERVER)\download
	
copy_wrk_area:
	@echo ���������������������������������������������������������������������ͻ
	@echo �            Copy doCID binary on Synergy workarea ...                �
	@echo ���������������������������������������������������������������������ͼ
	@mv doCID_v$(VERSION)_install.exe $(WORKAREA)
	
all: gui cli doc

easyig:
	@echo ���������������������������������������������������������������������ͻ
	@echo �            easyIG windows mode executable generation ...            �
	@echo ���������������������������������������������������������������������ͼ
	@$(PYTHON) setup_easyig.py py2exe
	
easyig_nsis: easyig
	@echo ���������������������������������������������������������������������ͻ
	@echo �            easyIG installer generation ...                          �
	@echo ���������������������������������������������������������������������ͼ
	@$(PATHNSIS)$(MAKENSIS) create_install_easyig.nsi

#
# This part convert markdown document readme.md into html document readme.html
#

# Implicit rules
.SUFFIXES: .html .md
.md.html:
	$(PYTHON) $(MARKDOWN) $< > $@

readme: readme.html
target: $(MD).html
#
# Launch tests
#
test:
	@echo ����������������ͻ
	@echo � Test _getItems �
	@echo ����������������ͼ
	$(DIST)\$(CLI_EXE) --cli -system Dassault_F5X_PDS -item "ESSNESS" -release SW_ENM/02 -baseline SW_ENM_DELIV_02_01 -cr_type SW_ENM
	@echo �������������ͻ
	@echo � Test _getCR �
	@echo �������������ͼ
	$(DIST)\$(CLI_EXE) --cli -system Dassault_F5X_PDS -item "ESSNESS" -release SW_ENM/02 -cr_type SW_ENM
clean:
	@rm -f -r -v $(DIST)/result/*.*
	@rm -f -v $(DIST)/*.exe
	@rm -f -v $(DIST)/*.py
	@rm -f -v $(DIST)/*.pyd
	@rm -f -v $(DIST)/*.html
	@rm -f -v $(DIST)/*.db3
	@rm -f -v $(DIST)/*.ico
	@rm -f -v $(DIST)/*.txt
	@rm -f -v $(DIST)/*.log
	@touch $(DIST)/result/empty.txt
