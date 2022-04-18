# Maintainer: Amina Khakimova <hakami1024@gmail.com>
# Contributor: Marcel Campello Ferreira <marcel.campello.ferreira@gmail.com>
# Contributor: Mark Dixon <mark@markdixon.name>
pkgname=neo4j-community
pkgver=4.4.5
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
install=neo4j.install
source=(http://dist.neo4j.org/neo4j-community-$pkgver-unix.tar.gz
        neo4j.conf
	neo4j.install
        neo4j.sysuser
        neo4j.service)
sha256sums=('26a3fa9f3c567a9edd337e2a33c82cc25e9ee65ca83e69ba52af628b96abb745'
            'a15570b179dce85e695ae4f98bf6784a93f8eb120a2690ffcc43b5c83e7c7417'
	    'e95540f908ebc41f88bf1c61befb5df9434a804f5c89abced65286302cf2dafa'
            '67a4ea5dc27c805bfcd3efb02f851856425e1d1beac80741a45b8233daf2f500'
	    'e39f383a26a0e50dff9760af8f9cf6b63213de10c0f898ddfb3661aa1fed828b')
package() {
  ############################
  # Directories as in Debian/RPM (excluding conf., which was not working): https://neo4j.com/docs/operations-manual/current/configuration/file-locations/
  ############################
  BIN_DIR=usr/bin
  CONFIG_DIR=usr/share/neo4j/conf
  DATA_DIR=var/lib/neo4j/data
  IMPORT_DIR=var/lib/neo4j/import
  LABS_DIR=usr/share/neo4j/labs
  LIB_DIR=usr/share/neo4j/lib
  LICENSES_DIR=usr/share/neo4j/licenses
  LOG_DIR=var/log/neo4j
  METRICS_DIR=var/lib/neo4j/metrics
  PLUGINS_DIR=var/lib/neo4j/plugins
  RUN_DIR=run/neo4j
  # Additional
  USR_BIN_DIR=usr/share/neo4j/bin
  DOC_DIR=usr/share/doc/neo4j

  ############################
  # Install directories with files
  ############################

  # Read-only directories for user
  install -d -m 755 $pkgdir/$CONFIG_DIR
  install -d -m 755 $pkgdir/$IMPORT_DIR
  install -d -m 755 $pkgdir/$USR_BIN_DIR #currently workaround
  install -d -m 755 $pkgdir/$BIN_DIR
  install -d -m 755 $pkgdir/$LIB_DIR
  install -d -m 755 $pkgdir/$PLUGINS_DIR
  install -d -m 755 $pkgdir/$CONFIG_DIR/certificates
  install -d -m 755 $pkgdir/$LICENSES_DIR

  # Read-write
  install -d -m 775 $pkgdir/$DATA_DIR
  install -d -m 775 $pkgdir/$LOG_DIR
  install -d -m 775 $pkgdir/$METRICS_DIR
  install -d -m 775 $pkgdir/$RUN_DIR

  # Additional
  install -d -m 755 $pkgdir/$DOC_DIR

  ############################
  # Install files
  ############################
  NEO4JSRC=$srcdir/neo4j-community-$pkgver

  # Install direct files
  install -D $srcdir/neo4j.conf -t $pkgdir/$CONFIG_DIR
  install -D $NEO4JSRC/README.txt $NEO4JSRC/UPGRADE.txt -t $pkgdir/$DOC_DIR
  install -D $NEO4JSRC/LICENSE.txt $NEO4JSRC/LICENSES.txt $NEO4JSRC/NOTICE.txt -t $pkgdir/$LICENSES_DIR

  [[ $(ls -A $NEO4JSRC/logs/* 2>/dev/null) ]] && cp -r $NEO4JSRC/logs/* $pkgdir/$LOG_DIR
  [[ $(ls -A $NEO4JSRC/lib/* 2>/dev/null) ]] && cp -r $NEO4JSRC/lib/* $pkgdir/$LIB_DIR
  [[ $(ls -A $NEO4JSRC/plugins/* 2>/dev/null) ]] && cp -r $NEO4JSRC/plugins/* $pkgdir/$PLUGINS_DIR
  [[ $(ls -A $NEO4JSRC/import/* 2>/dev/null) ]] && cp -r $NEO4JSRC/import/* -t $pkgdir/$IMPORT_DIR

  # Executable files
  install -D -m 644 $srcdir/neo4j.service $pkgdir/usr/lib/systemd/system/neo4j.service

  install -D -m 644 $srcdir/neo4j.sysuser $pkgdir/usr/lib/sysusers.d/neo4j.conf
  install -D -m 775 $NEO4JSRC/bin/cypher-shell $NEO4JSRC/bin/neo4j $NEO4JSRC/bin/neo4j-admin -t $pkgdir/$USR_BIN_DIR
  install -D -m 775 $NEO4JSRC/bin/tools/cypher-shell.jar -t $pkgdir/$USR_BIN_DIR
  for file in $(find $pkgdir/$USR_BIN_DIR -maxdepth 1 -type f); do
    b_file=$(basename $file)
    ln -s /$USR_BIN_DIR/$b_file $pkgdir/$BIN_DIR/$b_file;
  done
}
