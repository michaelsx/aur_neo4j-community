# Maintainer: Amina Khakimova <hakami1024@gmail.com>
# Contributor: Marcel Campello Ferreira <marcel.campello.ferreira@gmail.com>
# Contributor: Mark Dixon <mark@markdixon.name>
pkgname=neo4j-community
pkgver=4.4.2
pkgrel=1
pkgdesc='A fully transactional graph database implemented in Java'
arch=(any)
url=http://neo4j.org/
license=(custom)
#makedepends=(patch)
depends=('jre11-openjdk-headless')
conflicts=(neo4j-enterprise)
backup=(usr/share/neo4j/conf/neo4j.conf)
options=(!strip)
#install=neo4j.install
source=(http://dist.neo4j.org/neo4j-community-$pkgver-unix.tar.gz
        neo4j.conf
        neo4j.service)
sha256sums=('2594e5af77a9fad2338febe3b868d17dbd40fe537455758d19780d88621c8387'
            '1d0bff4f2074441958b2019d531741f0728f0450f3775a2810e1d226f1d49547'
            '002e1689b5707e481083d5d3a87e164ca4774af885962e38c657258ba29a4b51')
package() {

  # Directories as in Debian/RPM: https://neo4j.com/docs/operations-manual/current/configuration/file-locations/
  BIN_DIR=usr/bin
  #Config dir seems to be adapted
  #CONFIG_DIR=etc/neo4j
  CONFIG_DIR=usr/share/neo4j/conf
  DATA_DIR=var/lib/neo4j/data
  IMPORT_DIR=var/lib/neo4j/import
  LABS_DIR=usr/share/neo4j/labs
  LIB_DIR=usr/share/neo4j/lib
  LICENSES_DIR=usr/share/neo4j/licenses
  LOGS_DIR=var/log/neo4j
  METRICS_DIR=var/lib/neo4j/metrics
  PLUGINS_DIR=var/lib/neo4j/plugins
  RUN_DIR=var/lib/neo4j/run

  # Install directories 

  cd $srcdir/neo4j-community-$pkgver

  # Config files
  install -dm755 $pkgdir/$CONFIG_DIR
  install -dm700 $pkgdir/$CONFIG_DIR/certificates
  install -Dm644 $srcdir/neo4j.conf $pkgdir/$CONFIG_DIR/neo4j.conf

  # Data, import and log files
  #DATA_DIR=var/lib/neo4j/data
  install -dm755 $pkgdir/$DATA_DIR
  [[ $(ls -A data/* 2>/dev/null) ]] && cp -r data/* $pkgdir/$DATA_DIR

  #IMPORT_DIR=var/lib/neo4j/import
  install -dm755 $pkgdir/$IMPORT_DIR
  [[ $(ls -A import/* 2>/dev/null) ]] && cp -r import/* $pkgdir/$IMPORT_DIR

  #LOG_DIR=var/log/neo4j
  install -dm755 $pkgdir/$LOG_DIR
  [[ $(ls -A logs/* 2>/dev/null) ]] && cp -r logs/* $pkgdir/$LOG_DIR

  # Copy JARs in lib and plugins
  #LIB_DIR=usr/share/java/neo4j/lib
  install -dm755 $pkgdir/$LIB_DIR
  [[ $(ls -A lib/* 2>/dev/null) ]] && cp -r lib/* $pkgdir/$LIB_DIR

  #PLUGINS_DIR=usr/share/java/neo4j/plugins
  install -dm755 $pkgdir/$PLUGINS_DIR
  [[ $(ls -A plugins/* 2>/dev/null) ]] && cp -r plugins/* $pkgdir/$PLUGINS_DIR

  # Executable files
  USR_BIN_DIR=usr/share/neo4j/bin
  install -dm755 $pkgdir/$USR_BIN_DIR
  [[ $(ls -A bin/* 2>/dev/null) ]] && cp -r bin/* $pkgdir/$USR_BIN_DIR

  #SYSTEM_BIN_DIR=usr/bin
  install -dm755 $pkgdir/$BIN_DIR
  for file in $(find $pkgdir/$USR_BIN_DIR -maxdepth 1 -type f); do
    b_file=$(basename $file)
    ln -s /$USR_BIN_DIR/$b_file $pkgdir/$BIN_DIR/$b_file;
  done

  # Documentation
  DOC_DIR=usr/share/doc/neo4j
  install -dm755 $pkgdir/$DOC_DIR
  cp README.txt UPGRADE.txt $pkgdir/$DOC_DIR

  # License files
  #LICENSES_DIR=usr/share/licenses/neo4j
  install -dm755 $pkgdir/$LICENSES_DIR
  cp LICENSE.txt LICENSES.txt NOTICE.txt $pkgdir/$LICENSES_DIR

  # Service definition files
  install -Dm644 $srcdir/neo4j.service $pkgdir/usr/lib/systemd/system/neo4j.service

  # Runtime files
  #install -Dm644 $srcdir/neo4j-tmpfile.conf $pkgdir/usr/lib/tmpfiles.d/neo4j.conf
  install -dm755 $pkgdir/$RUN_DIR
}
