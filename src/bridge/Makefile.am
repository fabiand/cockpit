
noinst_LIBRARIES += \
	libcockpit-bridge.a \
	libcockpit-pcp.a \
	$(NULL)

libcockpit_bridge_a_SOURCES = \
	src/bridge/cockpitchannel.c \
	src/bridge/cockpitchannel.h \
	src/bridge/cockpitdbuscache.c \
	src/bridge/cockpitdbuscache.h \
	src/bridge/cockpitdbusinternal.h \
	src/bridge/cockpitdbusinternal.c \
	src/bridge/cockpitdbusjson.c \
	src/bridge/cockpitdbusjson.h \
	src/bridge/cockpitdbusrules.c \
	src/bridge/cockpitdbusrules.h \
	src/bridge/cockpitdbustime.c \
	src/bridge/cockpitdbususer.c \
	src/bridge/cockpitechochannel.c \
	src/bridge/cockpitechochannel.h \
	src/bridge/cockpithttpstream.c \
	src/bridge/cockpithttpstream.h \
	src/bridge/cockpitinteracttransport.c \
	src/bridge/cockpitinteracttransport.h \
	src/bridge/cockpitpackages.c \
	src/bridge/cockpitpackages.h \
	src/bridge/cockpitpaths.c \
	src/bridge/cockpitpaths.h \
	src/bridge/cockpitpolkitagent.c \
	src/bridge/cockpitpolkitagent.h \
	src/bridge/cockpitportal.c \
	src/bridge/cockpitportal.h \
	src/bridge/cockpitmetrics.c \
	src/bridge/cockpitmetrics.h \
	src/bridge/cockpitnullchannel.c \
	src/bridge/cockpitnullchannel.h \
	src/bridge/cockpitstream.c \
	src/bridge/cockpitstream.h \
	src/bridge/cockpitfsread.c \
	src/bridge/cockpitfsread.h \
	src/bridge/cockpitfsreplace.c \
	src/bridge/cockpitfsreplace.h \
	src/bridge/cockpitfswatch.c \
	src/bridge/cockpitfswatch.h \
	src/bridge/cockpitfslist.c \
	src/bridge/cockpitfslist.h \
	src/bridge/deprecated/cockpitdbusjson1.c \
	src/bridge/deprecated/cockpitdbusjson1.h \
	$(NULL)

libcockpit_bridge_a_CFLAGS = \
	-I$(srcdir)/src/bridge \
	-DG_LOG_DOMAIN=\"cockpit-bridge\" \
	$(COCKPIT_BRIDGE_CFLAGS) \
	$(NULL)

libcockpit_bridge_LIBS = \
	libcockpit-bridge.a \
	libcockpit-common.a \
	libwebsocket.a \
	$(COCKPIT_BRIDGE_LIBS) \
	$(NULL)

libcockpit_pcp_a_SOURCES = \
	src/bridge/cockpitchannel.c \
	src/bridge/cockpitchannel.h \
	src/bridge/cockpitmetrics.c \
	src/bridge/cockpitmetrics.h \
	src/bridge/cockpitpcpmetrics.c \
	src/bridge/cockpitpcpmetrics.h \
	$(NULL)

libcockpit_pcp_a_CFLAGS = \
	-I$(srcdir)/src/bridge \
	-DG_LOG_DOMAIN=\"cockpit-pcp\" \
	$(COCKPIT_PCP_CFLAGS) \
	$(NULL)

libcockpit_pcp_LIBS = \
	libcockpit-pcp.a \
	libcockpit-common.a \
	$(COCKPIT_PCP_LIBS) \
	$(NULL)

# -----------------------------------------------------------------------------

bin_PROGRAMS += cockpit-bridge
libexec_PROGRAMS += cockpit-polkit cockpit-pcp

cockpit_bridge_SOURCES = src/bridge/bridge.c
cockpit_bridge_CFLAGS = \
	-I$(srcdir)/src/bridge \
	-DG_LOG_DOMAIN=\"cockpit-bridge\" \
	$(COCKPIT_BRIDGE_CFLAGS) \
	$(NULL)
cockpit_bridge_LDADD = $(libcockpit_bridge_LIBS)

cockpit_pcp_SOURCES = src/bridge/cockpitpcp.c
cockpit_pcp_CFLAGS = \
	-DG_LOG_DOMAIN=\"cockpit-pcp\" \
	$(COCKPIT_PCP_CFLAGS) \
	$(NULL)
cockpit_pcp_LDADD = $(libcockpit_pcp_LIBS)

cockpit_polkit_SOURCES = src/bridge/cockpitpolkithelper.c
cockpit_polkit_CFLAGS = $(COCKPIT_POLKIT_CFLAGS)
cockpit_polkit_LDADD = libreauthorize.a $(REAUTHORIZE_LIBS) $(COCKPIT_POLKIT_LIBS)

# polkit-agent-helper-1 need to be setuid root because polkit wants
# responses to come from a root process
install-data-hook::
	-chown root:0 $(DESTDIR)$(libexecdir)/cockpit-polkit
	-chown root:0 $(DESTDIR)$(libexecdir)/cockpit-polkit
	-chmod 4755 $(DESTDIR)$(libexecdir)/cockpit-polkit

EXTRA_DIST += \
	src/bridge/cockpit.pam.insecure \
	src/bridge/sshd-reauthorize.pam \
	$(NULL)

# -----------------------------------------------------------------------------
# TESTS

BRIDGE_CHECKS = \
	test-paths \
	test-rules \
	test-channel \
	test-dbusjson \
	test-stream \
	test-packages \
	test-fs \
	test-metrics \
	test-pcp \
	test-pcp-archives \
	test-httpstream \
	$(NULL)

noinst_DATA += mock-pmda.so

CLEANFILES += mock-pmda.so

# This is non-portable, but I don't feel like dragging in libtool just
# for this.
#
mock-pmda.so: src/bridge/mock-pmda.c
	$(AM_V_CCLD) gcc -g -fPIC -shared \
		-DSRCDIR=\"$(abs_srcdir)\" \
		-o mock-pmda.so $(srcdir)/src/bridge/mock-pmda.c -lpcp_pmda -lpcp

test_channel_SOURCES = \
	src/bridge/test-channel.c \
	src/bridge/mock-transport.c src/bridge/mock-transport.h
test_channel_CFLAGS = $(libcockpit_bridge_a_CFLAGS)
test_channel_LDADD = $(libcockpit_bridge_LIBS)

test_dbusjson_SOURCES = src/bridge/deprecated/test-dbusjson.c $(mock_dbus_sources)
nodist_test_dbusjson_SOURCES = $(test_built_sources)
test_dbusjson_CFLAGS = $(libcockpit_bridge_a_CFLAGS)
test_dbusjson_LDADD = $(libcockpit_bridge_LIBS)

test_packages_SOURCES = src/bridge/test-packages.c \
	src/bridge/mock-transport.c src/bridge/mock-transport.h
test_packages_CFLAGS = $(libcockpit_bridge_a_CFLAGS)
test_packages_LDADD = $(libcockpit_bridge_LIBS)

test_paths_SOURCES = src/bridge/test-paths.c
test_paths_CFLAGS = $(libcockpit_bridge_a_CFLAGS)
test_paths_LDADD = $(libcockpit_bridge_LIBS)

test_rules_SOURCES = src/bridge/test-rules.c
test_rules_CFLAGS = $(libcockpit_bridge_a_CFLAGS)
test_rules_LDADD = $(libcockpit_bridge_LIBS)

test_stream_SOURCES = \
	src/bridge/test-stream.c \
	src/bridge/mock-transport.c src/bridge/mock-transport.h
test_stream_CFLAGS = $(libcockpit_bridge_a_CFLAGS)
test_stream_LDADD = $(libcockpit_bridge_LIBS)

test_fs_SOURCES = \
	src/bridge/test-fs.c \
	src/bridge/mock-transport.c src/bridge/mock-transport.h
test_fs_CFLAGS = $(libcockpit_bridge_a_CFLAGS)
test_fs_LDADD = $(libcockpit_bridge_LIBS)

test_metrics_SOURCES = \
	src/bridge/test-metrics.c \
	src/bridge/mock-transport.c src/bridge/mock-transport.h
test_metrics_CFLAGS = $(libcockpit_bridge_a_CFLAGS)
test_metrics_LDADD = $(libcockpit_bridge_LIBS)

test_pcp_SOURCES = \
	src/bridge/test-pcp.c \
	src/bridge/mock-transport.c src/bridge/mock-transport.h
test_pcp_CFLAGS = $(libcockpit_pcp_a_CFLAGS)
test_pcp_LDADD = $(libcockpit_pcp_LIBS) -ldl

test_pcp_archives_SOURCES = \
	src/bridge/test-pcp-archives.c \
	src/bridge/mock-transport.c src/bridge/mock-transport.h
test_pcp_archives_CFLAGS = $(libcockpit_pcp_a_CFLAGS)
test_pcp_archives_LDADD = $(libcockpit_pcp_LIBS) -ldl -lpcp_import

test_httpstream_SOURCES = \
	src/bridge/test-httpstream.c \
	src/bridge/mock-transport.c src/bridge/mock-transport.h
test_httpstream_CFLAGS = $(libcockpit_bridge_a_CFLAGS)
test_httpstream_LDADD = $(libcockpit_bridge_LIBS) -ldl

noinst_PROGRAMS += $(BRIDGE_CHECKS)
TESTS += $(BRIDGE_CHECKS)

EXTRA_DIST += \
	src/bridge/mock-resource \
	src/bridge/mock-pmda.c \
	src/bridge/mock-pmns \
	$(NULL)
