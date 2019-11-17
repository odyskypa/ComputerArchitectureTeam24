# Αρχιτεκτονική Υπολογιστών 7ο Εξάμηνο
### Κυπαρίσσης Οδυσσέας ΑΕΜ : 8955
### Αριστοτέλειο Πανεπιστήμιο Θεσσαλονίκης
### Τμήμα Ηλεκτρολόγων Μηχανικών και Μηχανικών Υπολογιστών  




# 1η Εργαστηριακή Άσκηση


**1)**
Κατά την εξομοίωση του συστήματος για το παράδειγμα Hello World χρησιμοποιείται το αρχείο starter_se.py το οποίο περνάει κάποιες βασικές παραμέτρους στο gem5 έτσι ώστε να γίνει η προσομοίωση με κάποια συγκεκριμένα χαρακτηριστικά συστήματος.Διαβάζοντας προσεκτικά το αρχείο starter_se.py παρατηρώ πως οι παράμετροι αυτοί είναι οι εξής.

- **Commands_to_run** : Αναφέρεται στο σύνολο των εντολών που θα τρέξουν στην προσομοίωση.

- **--cpu-type** : Αναφέρεται στον τύπο του επεξεργαστή(atomic, minor, hpi, TimingSimple, etc.) ο οποίος θα χρησιμοποιηθεί στην προσωμοίωση. Κάθε διαφορετικό είδος διαθέτει διαφορετική διάταξη των caches του συστήματος πιο συγκεκριμένα επιρεάζονται οι εξής οντότητες του συστήματος : l1_icache_class , l1_dcache_class, walk_cache_class και l2_Cache_class, (default="atomic" με όλες τις cache να είναι None).

- **cache_line_size** : Αναφέρεται στο μέγεθος γραμμής της μνήμης cache, και ορίζεται ίσο με 64bit.

- **voltage_domain** : Αναφέρεται στην τάση των συστατικών του συστήματος, (=3.3V).

- **clk_domain** : Αναφέρεται στην συχνότητα αυτών των συστατικών του συστήματος, (=1GHz)

- **--cpu-freq** : Αναφέρεται στην συχνότητα λειτουργίας του επεξεργαστή που θα χρησιμοποιηθεί,(default=4GHz).

- **--num-cores** : Αναφέρεται στον αριρμό των πυρήνων του επεξεργαστή που θα χρησιμοποιηθούν, (default = 1).

- **--mem-type** : Αναφέρεται στον τύπο της RAM που θα χρησιμοποιηθεί , δηλαδή στην ανάλογη τεχνολογία ,την συχνότητα λειτουργίας της αλλά και το μέγεθος της όπως για παράδειγμα DDR3_1600_4χ4 ή DDR4_2400_8x8, (default = DDR3_1600_8x8).

- **--mem-channels** : Αναφέρεται στον αριθμό καναλιών δεδομένων της μνήμης, (default = 2).

- **--mem-ranks** : Αναφέρεται στον αριθμό των επιπέδων της μνήμης RAM , τα οποία είναι blocks των 64 bit, (default = None).

- **--mem-size** : Αναφέρεται στο φυσικό μέγεθος της μνήμης, (default = 2GB).

**2)**
Τα παραγόμενα αρχεία ***stats.txt***, ***config.ini*** ή ***config.json*** από την προσομοίωση παρέχουν πληροφορίες για τους χρόνους εκτέλεσης αλλά και για το σύστημα που χρησιμοποίηθηκε. Όσον αφορά την εξομοίωση του Hello World στο πρώτο ερώτημα με ρυθμίσεις που ορίζει το αρχείο **starter_se.py** εξάγουμε τις εξής πληροφορίες:

**Από το αρχείο config.ini ή config.json** :

	[system]
	cache_line_size=64
	mem_mode=timing

	[system.clk_domain]
	type=SrcClockDomain
	clock=1000 # CPU-FREQ = 1/ clock * 10^(-12)
	voltage_domain=system.voltage_domain

	[system.cpu_cluster.cpus]
	type=MinorCPU
	executeMaxAccessesInMemory=2
	executeMemoryCommitLimit=1
	executeMemoryIssueLimit=1
	executeMemoryWidth=0

	[system.cpu_cluster.cpus.dcache]
	type=Cache
	assoc=2
	clk_domain=system.cpu_cluster.clk_domain
	default_p_state=UNDEFINED
	size=32768

	[system.cpu_cluster.cpus.dtb_walker_cache]
	type=Cache
	clk_domain=system.cpu_cluster.clk_domain
	size=1024

	[system.cpu_cluster.cpus.icache]
	type=Cache
	assoc=3
	clk_domain=system.cpu_cluster.clk_domain
	size=49152

	[system.cpu_cluster.cpus.icache.tags]
	type=BaseSetAssoc
	assoc=3
	block_size=64
	try_size=64
	size=49152

	[system.voltage_domain]
	type=VoltageDomain
	eventq_index=0
	voltage=3.3

	[system.mem_ctrls0]
	type=DRAMCtrl
	bank_groups_per_rank=0
	banks_per_rank=8
	burst_length=8
	clk_domain=system.clk_domain
	device_size=536870912
	ranks_per_channel=2
	read_buffer_size=32

**Από το αρχείο stats.txt** :

	sim_freq                                 1000000000000                       # Frequency of simulated ticks
	final_tick                                   24087000                       # Number of ticks from beginning of simulation (restored from checkpoints and never reset)


Όπως φαίνεται τα χαρακτηριστικά που πέρασαν στον gem5 μέσω του **starter_se.py** εμφανίζονται στα αρχεία που αναφέρθηκαν.

**3)**
Ο gem5 έχει την δυνατότητα να κάνει προσομοιώσεις με 2 διαφορετικά μοντέλα σειριακών επεξεργαστών (in-order CPUs) τα οποία αναφέρονται παρακάτω :  
Το πρώτο μοντέλο ονομάζεται *MinorCPU Model* και χρησιμοποιήθηκε στο ερώτημα (1).Το μοντέλο αυτό προσομοιώνει έναν σειριακό επεξεργαστή (in-order CPU) με σταθερό pipeline αλλά ρυθμιζόμες δομές δεδομένων  και συμπεριφορές εκτέλεσης.Το συγκεκριμένο μοντέλο προβλέπεται να χρησιμοποιείται για την μοντελοποίηση αυστηρά σειριακών συμπεριφορών εκτέλεσης και επιτρέπει την απεικόνιση της θέσης μιας εντολής στο εσωτερικό του pipeline μέσω του εργαλείου MinorTrace/minorview.py format/tool. Ο σκοπός του είναι να παρέχει μια δομή η οποία να συσχετίζεται, σε μικρο-αρχιτεκτονικό επίπεδο, με έναν συγκεκριμένο επεξεργαστή με παρόμοιες δυνατότητες.   
Το δεύτερο μοντέλο ονομάζεται *SimpleCPU Model* το οποίο αποτελείται απο 3 διαφορετικά υπο-μοντέλα(κλάσεις) με ονόματα: *BaseSimpleCPU*, *AtomicSimpleCPU* και *TimingSimpleCPU*. Το *SimpleCPU Μodel* ειναί αποκλειστικά ένα λειτουργικό, σειριακό μοντέλο, κατάλληλο για περιπτώσεις όπου ένα λεπτομεριακό μοντέλο δεν είναι απαραίτητο. Μπορεί να περιλαμβάνει warm-up periods , client systems τα οποία καθοδηγούν έναν διακομιστή ή μπορεί απλά να χρησιμοποιείται για τον έλεγχο σωστής λειτουργίας ενός προγράμματος.  
Η κλάση *BaseSimpleCPU* εξυπηρετεί στην υλοποίηση διαφόρων σκοπών.Αρχικά κρατάει το architected state , ορίζει συναρτήσεις για τον έλεγχο των interrupts , δημιουργεί αιτήματα απο fetches , διαχειρίζεται την pre-execute εγγατάσταση καθώς και τις post-execute ενέργειες και προωθεί τον PC στην επόμενη εντολή.Τέλος εφαρμόζει την ExecContext διεπαφή. Βέβαια ο *BaseSimpleCPU* δεν μπορεί να τρέξει ανεξάρτητος , αλλά πρέπει να χρησιμοποιείται μια απο τις κλάσεις που κληρονομούν απο την *BaseSimpleCPU* , είτε την *AtomicSimpleCPU* είτε την *TimingSimpleCPU*.  
Το μοντέλο *AtomicSimpleCPU* είναι η έκδοση του *SimpleCPU* η οποία χρησιμοποιεί ατομικές προσπελάσεις στην μνήμη. Χρησιμοποιεί εκτιμήσεις καθυστέρισης (latency estimates) από τις ατομικές προσπελάσεις για να προσδιορίσει με αυτό τον τρόπο τον συνολικό χρόνο πρόσβασης στην cache μνήμη.Το μοντέλο αυτό προέρχεται απο το *BaseSimpleCPU* και εφαρμώζει συναρτήσεις οι οποίες διαβάζουν και γράφουν στην μνήμη ,αλλά και χρησιμοποιεί τα ticks τα οποία ορίζουν τι συμβαίνει σε κάθε κύκλο του επεξεργαστή.Τέλος ορίζει της είσοδο που χρησιμοποιείται για να συνδεθεί στην μνήμη και συνδέει τον επεξεργαστή με την cache μνήμη.  
Το *TimingSimpleCPU* είναι η έκδοση του SimpleCPU η οποία χρησιμοποιεί χρονικές προσπελάσεις στην μνήμη(timing memory accesses).Καθυστερεί τις προσπελάσεις στην cache μνήμη και περιμένει το σύστημα της μνήμης να ανταποκριθεί πρίν προχωρήσει.Όπως και το μοντέλο *AtomicSimpleCPU* ομοίως και το *TimingSimpleCPU* είναι παραγώμενο απο το *BaseSimpleCPU* και εφαρμώζει το ίδιο σετ συναρτήτήσεων. Ορίζει την θύρα που χρησιμοποιείται για σύνδεση στην μνήμη και συνδέει τον επεξεργαστή με την μνήμη cache.Επίσης ορίζει τις απαραίτητες συναρτήσεις για την διαχείρηση της απόκρισης της μνήμης στις "αποτυχημένες" προσπελάσεις.

**a)** Για την εκτέλεση του προγράμματος **div.c**, το οποίο δημιούργησα, με την χρήση διαφορετικών μοντέλων επεξεργαστών χρησιμοποιώ τις παρακάτω εντολές :

**TimingSimpleCPU** :

	./build/ARM/gem5.opt -d timingsimple_result configs/example/se.py --cmd=tests/test-progs/myprog_arm/myprog_arm --cpu-type=TimingSimpleCPU

**MinorCPU** :

	./build/ARM/gem5.opt -d minor_result configs/example/se.py --cmd=tests/test-progs/myprog_arm/myprog_arm --cpu-type=MinorCPU --caches

Επεξήγηση του κώδικα **div.c** αναγράφεται ως σχόλια μέσα στον κώδικα.

Αποτελέσματα προσομοίωσης για τους χρόνους εκτέλεσης από το αρχείο stats.txt :

**TimingSimpleCPU** :

	final_tick                                  598046000                       # Number of ticks from beginning of simulation (restored from checkpoints and never reset)
	host_inst_rate                                 125656                       # Simulator instruction rate (inst/s)
	host_mem_usage                                 672156                       # Number of bytes of host memory used
	host_op_rate                                   143425                       # Simulator op (including micro ops) rate (op/s)
	host_seconds                                     0.07                       # Real time elapsed on the host
	host_tick_rate                             9134452417                       # Simulator tick rate (ticks/s)
	sim_freq                                 1000000000000                       # Frequency of simulated ticks
	sim_insts                                        8214                       # Number of instructions simulated
	sim_ops                                          9388                       # Number of ops (including micro ops) simulated
	sim_seconds                                  0.000598                       # Number of seconds simulated
	sim_ticks                                   598046000                       # Number of ticks simulated
	system.cpu.numCycles                          1196092                       # number of cpu cycles simulated
	system.clk_domain.clock                          1000                       # Clock period in ticks
	system.mem_ctrls.avgMemAccLat                22206.12                       # Average memory access latency per DRAM burst

**MinorCPU** :

	final_tick                                   34756000                       # Number of ticks from beginning of simulation (restored from checkpoints and never reset)
	host_inst_rate                                  96784                       # Simulator instruction rate (inst/s)
	host_mem_usage                                 678808                       # Number of bytes of host memory used
	host_op_rate                                   111011                       # Simulator op (including micro ops) rate (op/s)
	host_seconds                                     0.09                       # Real time elapsed on the host
	host_tick_rate                              406533911                       # Simulator tick rate (ticks/s)
	sim_freq                                 1000000000000                       # Frequency of simulated ticks
	sim_insts                                        8265                       # Number of instructions simulated
	sim_ops                                          9489                       # Number of ops (including micro ops) simulated
	sim_seconds                                  0.000035                       # Number of seconds simulated
	sim_ticks                                    34756000                       # Number of ticks simulated
	system.cpu.numCycles                            69512                       # number of cpu cycles simulated
	system.clk_domain.clock                          1000                       # Clock period in ticks
	system.mem_ctrls.avgMemAccLat                25843.43                       # Average memory access latency per DRAM burst

**b)** Όπως φαίνεται στο ερώτημα (a) , οι περισσότερες μετρικές του συστήματος ανάμεσα στις δύο προσομοιώσεις του ίδιου προγράμματος διαφέρουν . Αρχικά παρατηρείται μεγάλη διαφορά ανάμεσα στον αριθμό τον ticks , ο οποίος ουσιαστικά αντιστοιχεί στον χρόνο προσομοίωσης. Ο *TimingSimpleCPU* έτρεξε περισσότερο χρόνο απο τον *MinorCPU* το οποίο είναι λογικό και η επεξήγηση έχει αναφερθεί στην αρχή του ερωτήματος (3). O *TimingSimpleCPU* καθυστερεί τις προσπελάσεις στην cache μνήμη και περιμένει το σύστημα της μνήμης να ανταποκριθεί πρίν προχωρήσει . Επίσης φαίνεται πως ο *TimingSimpleCPU* είχε μεγαλύτερο αριθμό κύκλων CPU, σχεδόν τον διπλάσιο, απ'ότι ο *MinorCPU* κυρίως για τον ίδιο λόγο που προαναφέρθηκε. Αντιθέτως παρατηρώντας τις μετρικές τύπου host_* οι οποίες δηλώνουν την επίδοση του gem5, παρατηρείται πως ο *ΤimingSimpleCPU* είχε πάλι μεγαλύτερο instruction rate από τον *MinorCPU* καθώς και tick_rate . Τέλος, μερικές μετρήσεις και στις δύο προσομοιώσεις παραμένουν ίσες, ή σχεδόν ίσες. Για παράδειγμα, η συχνότητα των ticks και στις δύο περιπτώσεις ειναι 1GHz όπως καθορίζεται στις ρυθμίσεις του αρχείου se.py, ομοίως και το Clock period in ticks. Επίσης σχεδόν ίδιος είναι ο αριθμός των εντολών που προσομοιώθηκαν, το οποίο έχει να κάνει με το ότι και στις δυο περιπτώσεις έτρεξε το ίδιο πρόγραμμα. Για τον ίδιο λόγο, σχεδόν ίδιος είναι ο αριθμός των bytes της μνήμης που χρησιμοποίησε ο gem5.

**c)**
Στο ερώτημα αυτό γίνεται ξανά η προσομοίωση του προγράμματος **div.c** με τα δυο μοντέλα CPU που χρησιμοποιήθηκαν και παραπάνω, με την μόνη διαφορά ότι τώρα χρησιμοποιήθηκε CPU-Clock ίσο με 0.5GHz και τεχνολογία μνήμης DDR4_2400_8x8 ενώ οι default τιμές στο ερώτημα (3α) ήταν 1GHz και DDR3_1600_8x8 αντίστοιχα.

Για την χρήση του gem5 πληκτρολογώ τις παρακάτω εντολές :

**TimingSimpleCPU** :

	./build/ARM/gem5.opt -d timingsimple_result_c configs/example/se.py --cmd=tests/test-progs/myprog_arm/myprog_arm --cpu-type=TimingSimpleCPU --cpu-clock=0.5GHz --sys-clock=0.5GHz --mem-type=DDR4_2400_8x8

**MinorCPU** :

	./build/ARM/gem5.opt -d minor_result_c configs/example/se.py --cmd=tests/test-progs/myprog_arm/myprog_arm --cpu-type=MinorCPU --cpu-clock=0.5GHz --sys-clock=0.5GHz --mem-type=DDR4_2400_8x8 --caches

Τα αντίστοιχα αποτελέσματα παραθέτονται παρακάτω :

**TimingSimpleCPU** :

	final_tick                                  721446000                       # Number of ticks from beginning of simulation (restored from checkpoints and never reset)
	host_inst_rate                                 186329                       # Simulator instruction rate (inst/s)
	host_mem_usage                                 672408                       # Number of bytes of host memory used
	host_op_rate                                   212548                       # Simulator op (including micro ops) rate (op/s)
	host_seconds                                     0.04                       # Real time elapsed on the host
	host_tick_rate                            16327616015                       # Simulator tick rate (ticks/s)
	sim_freq                                 1000000000000                       # Frequency of simulated ticks
	sim_insts                                        8214                       # Number of instructions simulated
	sim_ops                                          9388                       # Number of ops (including micro ops) simulated
	sim_seconds                                  0.000721                       # Number of seconds simulated
	sim_ticks                                   721446000                       # Number of ticks simulated
	system.cpu.numCycles                           360723                       # number of cpu cycles simulated
	system.clk_domain.clock                          2000                       # Clock period in ticks
	system.mem_ctrls.avgMemAccLat                21934.72                       # Average memory access latency per DRAM burst

**MinorCPU** :

	final_tick                                   59580000                       # Number of ticks from beginning of simulation (restored from checkpoints and never reset)
	host_inst_rate                                 131786                       # Simulator instruction rate (inst/s)
	host_mem_usage                                 678808                       # Number of bytes of host memory used
	host_op_rate                                   151095                       # Simulator op (including micro ops) rate (op/s)
	host_seconds                                     0.06                       # Real time elapsed on the host
	host_tick_rate                              948469394                       # Simulator tick rate (ticks/s)
	sim_freq                                 1000000000000                       # Frequency of simulated ticks
	sim_insts                                        8265                       # Number of instructions simulated
	sim_ops                                          9489                       # Number of ops (including micro ops) simulated
	sim_seconds                                  0.000060                       # Number of seconds simulated
	sim_ticks                                    59580000                       # Number of ticks simulated
	system.cpu.numCycles                            29790                       # number of cpu cycles simulated
	system.clk_domain.clock                          2000                       # Clock period in ticks
	system.mem_ctrls.avgMemAccLat                24773.16                       # Average memory access latency per DRAM burst

Όπως φαίνεται, η διαφορά στα αποτελέσματα είναι εμφανής. Λόγω της αλλαγής της συχνότητας λειτουργίας η προσομοίωση διαρκεί περισσότερο χρόνο και στα δύο μοντέλα των CPUs, αλλά υπάρχουν και οι ανάλογες αλλαγές στον αριθμό των κύκλων του CPU, στην περίοδο ρολογιού καθώς και στον αριθμό των ticks που προσομοιώθηκαν, εφόσον όλες αυτές οι έννοιες είναι ρητά συνδεδεμένες μεταξύ τους. Ο αριθμός των εντολών παρέμεινε ίδιος καθώς και το instruction rate και στις δύο περιπτώσεις.


**Πηγές Πληροφοριών** :

http://learning.gem5.org/book/part1/gem5_stats.html)

http://pages.cs.wisc.edu/~david/courses/cs752/Spring2015/gem5-tutorial/part1/

http://www.gem5.org/docs/html/minor.html

http://learning.gem5.org/book/part1/example_configs.html

http://gem5.org/Adding_a_New_CPU_Model

http://gem5.org/SimpleCPU

http://gem5.org/Documentation

http://gem5.org/Configuration_/_Simulation_Scripts

http://gem5.org/InOrder_Instruction_Schedules

http://gem5.org/InOrder_Pipeline_Stages

http://gem5.org/InOrder_Resource-Request_Model

http://gem5.org/InOrder

https://www.researchgate.net/publication/220244788_The_gem5_simulator

**Κριτική** :

Όσον αφορά την εργασία , θεωρώ πως ήταν αρκετά χρήσιμη ώστε να είμαστε προιδεασμένοι για την δουλειά που θα αντιμετωπίσουμε στο εργαστήριο . Πιο συγκεκριμένα , ήρθαμε σε μια πολύ μικρή επαφή με τον προσομοιωτή gem5 αλλά χρειαζόταν αρκετός χρόνος ώστε να γίνει πιο οικεία η αναζήτηση χρόνων και αποτελεσμάτων των προσομοιώσεων , το οποίο θεωρώ , πως είναι και ο σκοπός του εργαστηρίου .Δεν θεωρώ πως ήταν δύσκολη απλά χρειαζόταν αρκετό χρόνο εξερεύνησης σε ιστοσελίδες του gem5 και γενικότερα στην βιβλιογραφία . Το μόνο αρνητικό που θα ανέφερα είναι πως ενώ αρχικά έιχα dual boot linux - windows πριν ασχοληθώ με το μάθημα , επειδή οι εντολές που δίνονται στο architecture_lab1.pdf δεν δούλευαν επαρκώς στο elementaryOS distro που χρησιμοποιούσα , αναγκάστηκα να ξανα κάνω εγγατάσταση των ubuntu μέσω VM . Κατα την γνώμη μου , θα ήταν πιο σωστό να υποχρεώνονται όλοι οι φοιτητές να ακολουθούν ένα μοντέλο , ώστε να υπάρχει μεγαλύτερη σαφήνεια στις οδηγείες και να μην δημιουργούνται τέτοιου είδους θέματα , δηλαδή αν λέγατε ότι υποχρεωτικά θα πρέπει η εγκατάσταση του gem5 να γίνει μέσω VM ίσως αρκετοί φοιτητές να μην είχαν παρόμοια προβλήματα , εφόσον οι οδηγίες που δίνεται στο pdf δουλεύουν 100% σωστά στην έκδοση ubuntu 19.10. Παρ'όλα αυτά , είδα πως αναφέρατε ότι θα υπάρχει η αντίστοιχη βοήθεια αν υπάρχουν τέτοια προβλήματα , αλλά θεωρώ πως είναι χάσιμο χρόνου και για εμάς αλλά και για εσάς .
