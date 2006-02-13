MAKEFLAGS=-s -k
PRINTF = @PRINTF@
HOST = @host@
TARGET = @target@
BUILD = @build@
nic1 = @nic1@
nic2 = @nic2@
ENABLE_DEBUG = @debug_flag@
TCPPREP=../src/tcpprep --no-arg-comment
TCPREPLAY=../src/tcpreplay
TCPREWRITE=../src/tcprewrite
TCPBRIDGE=../src/tcpbridge

EXTRA_DIST = test.pcap test.auto_bridge test.auto_client test.auto_router \
		 test.auto_server test.cidr test.comment test.port \
		 test.prep_config test.primary test.regex test.secondary

test: all
all: clearlog check tcpprep tcpreplay

clearlog:
	-rm test.log

check:
	$(PRINTF) "%s\n" "NOTICE: Tests must be run as root"
	$(PRINTF) "%s\n" "Sending traffic on '$(nic1)' and '$(nic2)'"

# the cache files to be used as the standard; be sure to do a cvs commit
# afterwords!
standard:
	$(TCPPREP) -i test.pcap -o test.auto_router -a router
	$(TCPPREP) -i test.pcap -o test.auto_bridge -a bridge
	$(TCPPREP) -i test.pcap -o test.auto_client -a client
	$(TCPPREP) -i test.pcap -o test.auto_server -a server
	$(TCPPREP) --load-opts config -o test.prep_config
	$(TCPPREP) -i test.pcap -o test.port -p
	$(TCPPREP) -i test.pcap -o test.cidr -c '216.27.178.0/24'
	$(TCPPREP) -i test.pcap -o test.regex -r '216.27.178.*'
	$(TCPPREP) -i test.pcap -o test.comment -C "This is a comment" -p

tcpprep: auto_router auto_bridge auto_client auto_server cidr regex port comment print_info print_comment prep_config
tcprewrite: rewrite_cache rewrite_cidr rewrite_trunc rewrite_pad rewrite_rand rewrite_layer2 rewrite_config
tcpreplay: replay_basic replay_cache replay_pps replay_rate replay_top replay_config replay_multi

prep_config:
	$(PRINTF) "%s" "[tcpprep] Config mode test: "
	$(PRINTF) "%s\n" "*** [tcpprep] Config mode test: " >>test.log
	$(TCPPREP) $(ENABLE_DEBUG) --load-opts=config -o test.$@1 2>>test.log
	diff test.$@ test.$@1 >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t\t%s\n" "FAILED"; else $(PRINTF) "\t\t%s\n" "OK"; fi

auto_router:
	$(PRINTF) "%s" "[tcpprep] Auto/Router mode test: "
	$(PRINTF) "%s\n" "*** [tcpprep] Auto/Router mode test: " >>test.log
	$(TCPPREP) $(ENABLE_DEBUG) -i test.pcap -o test.$@1 -a router 2>>test.log
	diff test.$@ test.$@1 >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t%s\n" "FAILED"; else $(PRINTF) "\t%s\n" "OK"; fi

auto_bridge:
	$(PRINTF) "%s" "[tcpprep] Auto/Bridge mode test: "
	$(PRINTF) "%s\n" "*** [tcpprep] Auto/Bridge mode test: " >>test.log
	$(TCPPREP) $(ENABLE_DEBUG) -i test.pcap -o test.$@1 -a bridge 2>>test.log
	diff test.$@ test.$@1 >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t%s\n" "FAILED"; else $(PRINTF) "\t%s\n" "OK"; fi

auto_client:
	$(PRINTF) "%s" "[tcpprep] Auto/Client mode test: "
	$(PRINTF) "%s\n" "*** [tcpprep] Auto/Client mode test: " >>test.log
	$(TCPPREP) $(ENABLE_DEBUG) -i test.pcap -o test.$@1 -a client 2>>test.log
	diff test.$@ test.$@1 >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t%s\n" "FAILED"; else $(PRINTF) "\t%s\n" "OK"; fi

auto_server:
	$(PRINTF) "%s" "[tcpprep] Auto/Server mode test: "
	$(PRINTF) "%s\n" "*** [tcpprep] Auto/Server mode test: " >>test.log
	$(TCPPREP) $(ENABLE_DEBUG) -i test.pcap -o test.$@1 -a server 2>>test.log
	diff test.$@ test.$@1 >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t%s\n" "FAILED"; else $(PRINTF) "\t%s\n" "OK"; fi

regex:
	$(PRINTF) "%s" "[tcpprep] Regex mode test: "
	$(PRINTF) "%s\n" "*** [tcpprep] Regex mode test: " >>test.log
	$(TCPPREP) $(ENABLE_DEBUG) -i test.pcap -o test.$@1 -r '216.27.178.*' 2>>test.log
	diff  test.$@ test.$@1 >>test.log 2>&1 
	if [ $? ] ; then $(PRINTF) "\t\t%s\n" "FAILED"; else $(PRINTF) "\t\t%s\n" "OK"; fi

cidr:
	$(PRINTF) "%s" "[tcpprep] CIDR mode test: "
	$(PRINTF) "%s\n" "*** [tcpprep] CIDR mode test: " >>test.log
	$(TCPPREP) $(ENABLE_DEBUG) -i test.pcap -o test.$@1 -c '216.27.178.0/24' 2>>test.log
	diff test.$@ test.$@1 >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t\t%s\n" "FAILED"; else $(PRINTF) "\t\t%s\n" "OK"; fi

port:
	$(PRINTF) "%s" "[tcpprep] Port mode test: "
	$(PRINTF) "%s\n" "*** [tcpprep] Port mode test: " >>test.log
	$(TCPPREP) $(ENABLE_DEBUG) -i test.pcap -o test.$@1 -p 2>>test.log
	diff test.$@ test.$@1 >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t\t%s\n" "FAILED"; else $(PRINTF) "\t\t%s\n" "OK"; fi

comment:
	$(PRINTF) "%s" "[tcpprep] Comment mode test: "
	$(PRINTF) "%s\n" "*** [tcpprep] Comment mode test: " >>test.log
	$(TCPPREP) $(ENABLE_DEBUG) -i test.pcap -o test.$@1 -C "This is a comment" -p 2>>test.log
	if [ $? ] ; then $(PRINTF) "\t\t%s\n" "FAILED"; else $(PRINTF) "\t\t%s\n" "OK"; fi


print_comment:
	$(PRINTF) "%s" "[tcpprep] Print comment mode test: "
	$(PRINTF) "%s\n" "*** [tcpprep] Print comment mode test: " >>test.log
	$(TCPPREP) $(ENABLE_DEBUG) -P test.comment >test.$@1 2>>test.log
	if [ $? ] ; then $(PRINTF) "\t%s\n" "FAILED"; else $(PRINTF) "\t%s\n" "OK"; fi

print_info:
	$(PRINTF) "%s" "[tcpprep] Print info mode test: "
	$(PRINTF) "%s\n" "*** [tcpprep] Print info mode test: " >>test.log
	$(TCPPREP) $(ENABLE_DEBUG) -I test.comment >test.$@1 2>>test.log
	if [ $? ] ; then $(PRINTF) "\t%s\n" "FAILED"; else $(PRINTF) "\t%s\n" "OK"; fi

replay_basic:
	$(PRINTF) "%s" "[tcpreplay] Basic test: "
	$(PRINTF) "%s\n" "*** [tcpreplay] Basic test: " >>test.log
	$(TCPREPLAY) $(ENABLE_DEBUG) -i $(nic1) -t test.pcap >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t\t%s\n" "FAILED"; else $(PRINTF) "\t\t%s\n" "OK"; fi

replay_cache:
	$(PRINTF) "%s" "[tcpreplay] Cache test: "
	$(PRINTF) "%s\n" "*** [tcpreplay] Cache test: " >>test.log
	$(TCPREPLAY) $(ENABLE_DEBUG) -c test.cidr -i $(nic1) -I $(nic2) -t test.pcap >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t\t%s\n" "FAILED"; else $(PRINTF) "\t\t%s\n" "OK"; fi

rewrite_cidr:
	$(PRINTF) "%s" "[tcpreplay] CIDR test: "
	$(PRINTF) "%s\n" "*** [tcpreplay] CIDR test: " >>test.log
	$(TCPREPLAY) $(ENABLE_DEBUG) -C 216.27.178.0/24 -i $(nic1) -I $(nic2) -t test.pcap >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t\t\t%s\n" "FAILED"; else $(PRINTF) "\t\t\t%s\n" "OK"; fi

rewrite_trunc:
	$(PRINTF) "%s" "[tcpreplay] Truncate test: "
	$(PRINTF) "%s\n" "*** [tcpreplay] Truncate test: " >>test.log
	$(TCPREPLAY) $(ENABLE_DEBUG) -i $(nic1) -u trunc -t test.pcap >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t\t%s\n" "FAILED"; else $(PRINTF) "\t\t%s\n" "OK"; fi

rewrite_pad:
	$(PRINTF) "%s" "[tcpreplay] Pad test: "
	$(PRINTF) "%s\n" "*** [tcpreplay] Pad test: " >>test.log
	$(TCPREPLAY) $(ENABLE_DEBUG) -i $(nic1) -u pad -R test.pcap >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t\t\t%s\n" "FAILED"; else $(PRINTF) "\t\t\t%s\n" "OK"; fi

rewrite_rand:
	$(PRINTF) "%s" "[tcpreplay] Random IP test: "
	$(PRINTF) "%s\n" "*** [tcpreplay] Random IP test: " >>test.log
	$(TCPREPLAY) $(ENABLE_DEBUG) -i $(nic1) -s 666 -R test.pcap >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t\t%s\n" "FAILED"; else $(PRINTF) "\t\t%s\n" "OK"; fi

replay_include:
	$(PRINTF) "%s" "[tcpreplay] Include only test: "
	$(PRINTF) "%s\n" "*** [tcpreplay] Include only test: " >>test.log
	$(TCPREPLAY) $(ENABLE_DEBUG) -i $(nic1) -x P:1,5,7-53,88,93-100 -R test.pcap >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t\t%s\n" "FAILED"; else $(PRINTF) "\t\t%s\n" "OK"; fi

replay_exclude:
	$(PRINTF) "%s" "[tcpreplay] Exclude test: "
	$(PRINTF) "%s\n" "*** [tcpreplay] Exclude test: " >>test.log
	$(TCPREPLAY) $(ENABLE_DEBUG) -i $(nic1) -X P:1,5,7-53,88,93-100 -R test.pcap >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t\t%s\n" "FAILED"; else $(PRINTF) "\t\t%s\n" "OK"; fi

rewrite_layer2:
	$(PRINTF) "%s" "[tcpreplay] Layer2 test: "
	$(PRINTF) "%s\n" "*** [tcpreplay] Layer2 test: " >>test.log
	$(TCPREPLAY) $(ENABLE_DEBUG) -i $(nic1) -2 00,50,da,5d,46,55,0,7,eb,30,a4,c3,08,0 -R test.pcap >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t\t%s\n" "FAILED"; else $(PRINTF) "\t\t%s\n" "OK"; fi

replay_pps:
	$(PRINTF) "%s" "[tcpreplay] Packets/sec test: "
	$(PRINTF) "%s\n" "*** [tcpreplay] Packets/sec test: " >>test.log
	$(TCPREPLAY) $(ENABLE_DEBUG) -i $(nic1) -p 25 test.pcap >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t\t%s\n" "FAILED"; else $(PRINTF) "\t\t%s\n" "OK"; fi


replay_rate:
	$(PRINTF) "%s" "[tcpreplay] Mbps test: "
	$(PRINTF) "%s\n" "*** [tcpreplay] Mbps test: " >>test.log
	$(TCPREPLAY) $(ENABLE_DEBUG) -i $(nic1) -M 25.0 test.pcap >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t\t\t%s\n" "FAILED"; else $(PRINTF) "\t\t\t%s\n" "OK"; fi


replay_multi:
	$(PRINTF) "%s" "[tcpreplay] Multiplier test: "
	$(PRINTF) "%s\n" "*** [tcpreplay] Multiplier test: " >>test.log
	$(TCPREPLAY) $(ENABLE_DEBUG) -i $(nic1) -m 25.0 test.pcap >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t\t%s\n" "FAILED"; else $(PRINTF) "\t\t%s\n" "OK"; fi

replay_top:
	$(PRINTF) "%s" "[tcpreplay] Topspeed test: "
	$(PRINTF) "%s\n" "*** [tcpreplay] Topspeed test: " >>test.log
	$(TCPREPLAY) $(ENABLE_DEBUG) -i $(nic1) -t test.pcap >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t\t%s\n" "FAILED"; else $(PRINTF) "\t\t%s\n" "OK"; fi

datadump_mode:
	$(PRINTF) "%s" "[tcpreplay] Data dump mode test: "
	$(PRINTF) "%s\n" "*** [tcpreplay] Data dump mode test: " >>test.log
	$(TCPREPLAY) $(ENABLE_DEBUG) -D -i $(nic1) -j $(nic2) -w primary.data -W secondary.data -c test.cidr -R test.pcap >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t%s\n" "FAILED"; else $(PRINTF) "\t%s\n" "OK"; fi

replay_config:
	$(PRINTF) "%s" "[tcpreplay] Config file mode test: "
	$(PRINTF) "%s\n" "*** [tcpreplay] Config file mode test: " >>test.log
	$(TCPREPLAY) $(ENABLE_DEBUG) --load-opts=config test.pcap >>test.log 2>&1
	if [ $? ] ; then $(PRINTF) "\t%s\n" "FAILED"; else $(PRINTF) "\t%s\n" "OK"; fi

clean:
	rm -f *1 test.log core* *~ primary.data secondary.data

distclean: clean
	rm -f Makefile config

maintainer-clean: distclean