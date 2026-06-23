# Top 10 Perguntas Básicas sobre Gerenciamento de Memória

Para explicação completa, consulte os [monitores]().

## Perguntas Básicas

<details>
    <summary>1. O que é a hierarquia de memória?</summary>
    <br><p>A hierarquia de memória organiza diferentes níveis de armazenamento, desde a memória cache (rápida e cara) até o armazenamento não volátil (lento e barato). O sistema operacional gerencia essa hierarquia para oferecer um modelo útil e eficiente aos programas. Graças à hierarquia de memória, os programas podem aproveitar tanto de memórias mais rápidas, como a memória primária (RAM), quanto de memória secundária que possui mais espaço (HDDs, SSDs). O gerenciador de memória é responsável por alocar e desalocar memória conforme necessário.</p>
    <div style="background-color: #EEE;">
        <span>Referência na página 208 (ou 179) do livro Modern Operating Systems 5ª edição por Tanenbaum e Bos</span><br>
        <span style="color: #888;font-size: 0.8em;">
            What every programmer would like is a private, infinitely large, infinitely fast memory that is also nonvolatile, that is, does not lose its contents when the electric power is switched off. While we are at it, why not make it inexpensive, too? Unfortunately, technology does not provide such memories at present. Maybe you will discover how to do it.<br>

            What is the second choice? Over the years, people discovered the concept of a memory hierarchy, in which computers have a few meg abytes of very fast, expensive, volatile cache memory, a few gigabytes of medium-speed, medium-priced, volatile main memory, and a few terabytes of slow, cheap, nonvolatile magnetic or solid-state storage, not to mention removable storage, such as USB sticks. It is the job of the operating system to abstract this hierarchy into a useful model and then manage the abstraction.<br>

            The part of the operating system that manages (part of) the memory hierarchy is called the memory manager. Its job is to efficiently manage memory: keep track of which parts of memory are in use, allocate memory to processes when they need it, and deallocate it when they are done.
        </span>
    </div>
</details>

<details>
    <summary>2. O que é espaço de endereçamento?</summary>
    <br><p>Espaço de endereçamento é a abstração da memória principal. Ela é um o conjunto de endereços que um processo pode utilizar para acessar a memória. Cada processo possui seu próprio espaço de endereçamento, independente dos demais, criando uma abstração de memória privada. Isso permite que múltiplos programas coexistam sem interferirem uns nos outros.</p>
    <div style="background-color: #EEE;">
        <span>Referência na página 213 (ou 184) do livro Modern Operating Systems 5ª edição por Tanenbaum e Bos</span><br>
        <span style="color: #888;font-size: 0.8em;">Tw o problems have to be solved to allow multiple applications to be in memory at the same time without interfering with each other: protection and relocation. We looked at a primitive solution to the former used on the IBM 360: label chunks of memory with a protection key and compare the key of the executing process to that of every memory word fetched. However, this approach by itself does not solve the latter problem, although it can be solved by relocating programs as they are loaded, but this is a slow and complicated solution.<br>

        A better solution is to invent a new abstraction for memory: the address space. Just as the process concept creates a kind of abstract CPU to run programs, the address space creates a kind of abstract memory for programs to use. An address space is the set of addresses that a process can use to address memory. Each process has its own address space, independent of those of other processes (except in some special circumstances where processes want to share their address spaces).<br>

        Address spaces do not have to be numeric. The set of .com Internet domains is also an address space. This address space consists of all the strings of length 2 to 63 characters that can be made using letters, numbers, and hyphens, followed by .com. By now you should get the idea. It is fairly simple.<br>

        Somewhat harder is how to giv e each program its own address space, so address 28 in one program means a different physical location than address 28 in another program. Below we will discuss a simple way that used to be common but has fallen into disuse due to the ability to put much more complicated (and better) schemes on modern CPU chips.<br>
        </span>
    </div>
</details>

<details>
    <summary>3. O que é swapping?</summary>
    <br><p>Swapping é uma técnica onde processos inteiros são trazidos para a memória, executados por um tempo e depois devolvidos ao armazenamento não volátil (disco ou SSD). Isso libera memória para outros processos, permitindo que o sistema lide com mais processos do que caberiam fisicamente na RAM. Processos inativos ficam armazenados em disco, não ocupando memória.</p>
    <div style="background-color: #EEE;">
        <span>Referência na página 215 (ou 187) do livro Modern Operating Systems 5ª edição por Tanenbaum e Bos</span><br>
        <span style="color: #888;font-size: 0.8em;">If the physical memory of the computer is large enough to hold all the processes, the schemes described so far will more or less do. But in practice, the total amount of RAM needed by all the processes is often much more than can fit in memory. On a typical Windows, MacOS, or Linux system, something like 50–100 processes or more may be started up as soon as the computer is booted. For example, when a Windows application is installed, it often issues commands so that on subsequent system boots, a process will be started that does nothing except check for updates to the application. Such a process can easily occupy 5–10 MB of memory. Other background processes check for incoming mail, incoming network connections, and many other things. And all this is before the first user program is started. Serious user application programs nowadays, like Photoshop, can require almost a gigabyte just to boot and many gigabytes once they start processing data. Consequently, keeping all processes in memory all the time requires a huge amount of memory and cannot be done if there is insufficient memory.

        Two general approaches to dealing with memory overload have been developed over the years. The simplest strategy, called swapping of processes, consists of bringing in each process in its entirety, running it for a while, then putting it back on nonvolatile storage (disk or SSD). Idle processes are mostly stored on nonvolatile storage, so they do not take up any memory when they are not running (although some of them wake up periodically to do their work, then go to sleep again). The other strategy, called virtual memory, allows programs to run even when they are only partially in main memory. Below we will study swapping; in Sec. 3.3 we will examine virtual memory.
        </span>
    </div>
</details>

<details>
    <summary>4. Do que se trata gerenciamento de memória livre?</summary>
    <br><p>Gerenciamento de memória livre envolve rastrear quais partes da memória estão ocupadas ou livres para alocação dinâmica. Existem duas abordagens principais: mapas de bits (bitmaps), onde cada unidade de alocação tem um bit indicando ocupação, e listas encadeadas (free lists), que mantêm segmentos de memória alocados e livres. O sistema operacional usa esses métodos para alocar e desalocar memória eficientemente.</p>
    <div style="background-color: #EEE;">
        <span>Referência na página 217 (ou 189) do livro Modern Operating Systems 5ª edição por Tanenbaum e Bos</span><br>
        <span style="color: #888;font-size: 0.8em;">
            When memory is assigned dynamically, the operating system must manage it. In general terms, there are two ways to keep track of memory usage: bitmaps and free lists. In this section and the next one, we will look at these two methods. In Chapter 10, we will look at some specific memory allocators used in Linux (like buddy and slab allocators) in more detail. We will also see in later chapters that tracking the usage of resources is not specific to memory management. For instance, file systems also need to keep track of free disk blocks. In fact, keeping track of what slots are free in an set of resources is common in many programs.

            With a bitmap, memory is divided into allocation units as small as a few words and as large as several kilobytes. Corresponding to each allocation unit is a bit in the bitmap, which is 0 if the unit is free and 1 if it is occupied (or vice versa). Figure 3-6(a) shows part of memory and the corresponding bitmap in Fig. 3-6(b).

            Another way of keeping track of memory is to maintain a linked list of allocated and free memory segments, where a segment either contains a process or is an empty hole between two processes. The memory of Fig. 3-6(a) is represented in Fig. 3-6(c) as a linked list of segments. Each entry in the list specifies a hole (H) or process (P), the address at which it starts, the length, and a pointer to the next item.
        </span>
    </div>
</details>

<details>
    <summary>5. O que é memória virtual?</summary>
    <br><p>Memória virtual é uma técnica que permite que cada programa tenha seu próprio espaço de endereçamento, dividido em páginas. Nem todas as páginas precisam estar na memória física ao mesmo tempo para o programa executar. Quando uma página não está presente, o sistema operacional a carrega do disco e reexecuta a instrução que falhou.</p>
    <div style="background-color: #EEE;">
        <span>Referência na página 221 (ou 193) do livro Modern Operating Systems 5ª edição por Tanenbaum e Bos</span><br>
        <span style="color: #888;font-size: 0.8em;">
        The method that was devised (Fotheringham, 1961) has come to be known as virtual memory. The basic idea behind virtual memory is that each program has its own address space, which is broken up into chunks called pages. Each page is acontiguous range of addresses. These pages are mapped onto physical memory, but not all pages have to be in physical memory at the same time to run the program. When the program references a part of its address space that is in physical memory, the hardware performs the necessary mapping on the fly. When the program references a part of its address space that is not in physical memory, the operating system is alerted to go get the missing piece and re-execute the instruction that failed.
        </span>
    </div>
</details>

<details>
    <summary>6. O que é paging?</summary>
    <br><p>Paging é a técnica de mapear endereços virtuais (gerados pelo programa) para endereços físicos (na memória RAM) usando uma Unidade de Gerenciamento de Memória (MMU). O espaço virtual é dividido em páginas de tamanho fixo, e a memória física em page frames do mesmo tamanho. A MMU traduz os endereços virtuais em físicos durante a execução.</p>
    <div style="background-color: #EEE;">
        <span>Referência na página 222 (ou 193) do livro Modern Operating Systems 5ª edição por Tanenbaum e Bos</span><br>
        <span style="color: #888;font-size: 0.8em;">Most virtual memory systems use a technique called paging, which we will now describe. On any computer, programs reference a set of memory addresses. When a program executes an instruction like MOV REG,1000 it does so to copy the contents of memory address 1000 to REG (assuming the first operand represents the destination and the second the source). Addresses can be generated using indexing, base registers, and various other ways.<br>

        These program-generated addresses are called virtual addresses and form the virtual address space. On computers without virtual memory, the virtual address is put directly onto the memory bus and causes the physical memory word with the same address to be read or written. When virtual memory is used, the virtual addresses do not go directly to the memory bus. Instead, they go to an MMU (Memory Management Unit)that maps the virtual addresses onto the physical memory addresses, as illustrated in Fig. 3-8.<br>

        The virtual address space consists of fixed-size units called pages. The corresponding units in the physical memory are called page frames. The pages and page frames are the same size. In this example they are 4 KB, but page sizes from 512 bytes to a gigabyte have been used in real systems. With 64 KB of virtual address space and 32 KB of physical memory, we get 16 virtual pages and 8 page frames. Transfers between RAM and nonvolatile storage are always in whole pages. Many processors support multiple page sizes that can be mixed and matched as the operating system sees fit. For instance, the x86-64 architecture supports 4-KB, 2-MB, and 1-GB pages, so we could use 4-KB pages for user applications and a single 1-GB page for the kernel. We will see later why it is sometimes better to use a single large page, rather than a large number of small ones.<br></span>
    </div>
</details>

<details>
    <summary>7. O que é um page fault?</summary>
    <br><p>Page fault é uma interrupção gerada pela MMU quando um programa tenta acessar uma página virtual que não está mapeada na memória física. O sistema operacional então seleciona um page frame pouco usado, salva seu conteúdo no disco se necessário, carrega a página solicitada do disco e atualiza os mapas de memória. Após isso, a instrução que causou a falha é reiniciada.</p>
    <div style="background-color: #EEE;">
        <span>Referência na página 224 (ou 196) do livro Modern Operating Systems 5ª edição por Tanenbaum e Bos</span><br>
        <span style="color: #888;font-size: 0.8em;">What happens if the program references an unmapped address, for example, by using the instruction 

        MOV REG,32780

        which is byte 12 within virtual page 8 (starting at 32768)? The MMU sees that the page is unmapped (indicated by a cross in the figure) and causes the CPU to trap to the operating system, called a page fault. The operating system picks a little-used page frame and writes its contents back to the disk (if it is not already there). It then fetches (also from the disk) the page that was just referenced into the page frame just freed, changes the map, and restarts the trapped instruction.</span>
    </div>
</details>

<details>
    <summary>8. Qual o problema de Translation Lookaside Buffer trata?</summary>
    <br><p>O TLB (Translation Lookaside Buffer) resolve o problema de performance causado por acessos à tabela de páginas na memória, que dobrariam o tempo de cada referência à memória. Ele é um cache de hardware pequeno e rápido que armazena mapeamentos recentes de páginas virtuais para físicas. Como os programas tendem a acessar poucas páginas repetidamente, o TLB acelera significativamente a tradução de endereços.</p>
    <div style="background-color: #EEE;">
        <span>Referência na página 230 (ou 201) do livro Modern Operating Systems 5ª edição por Tanenbaum e Bos</span><br>
        <span style="color: #888;font-size: 0.8em;">Let us now look at some widely implemented schemes for speeding up paging and for handling large virtual address spaces, starting with the former. The starting point of most optimization techniques is that the page table is in memory. Potentially, this design has an enormous impact on performance. Consider, for example, a 1-byte instruction that copies one register to another. In the absence of paging, this instruction makes only one memory reference, to fetch the instruction. With paging, at least one additional memory reference will be needed, to access the page table. Since execution speed is generally limited by the rate at which the CPU can get instructions and data out of the memory, having to make two memory references per memory reference reduces performance by half. Under these conditions, no one would use paging.<br>

        Computer designers have known about this problem for years and have come up with a solution. Their solution is based on the observation that most programs tend to make a large number of references to a small number of pages, and not the other way around. Thus only a small fraction of the page table entries are heavily read; the rest are barely used at all.<br>

        The solution that has been devised is to equip computers with a small hardware device for mapping virtual addresses to physical addresses without going through the page table. The device, called a TLB (Translation Lookaside Buffer) or sometimes an associative memory, is illustrated in Fig. 3-12. It is usually inside the MMU and consists of a small number of entries, eight in this example, but rarely more than 256. Each entry contains information about one page, including the virtual page number, a bit that is set when the page is modified, the protection code (read/write/execute permissions), and the physical page frame in which the page is located. These fields have a one-to-one correspondence with the fields in the page table, except for the virtual page number, which is not needed in the page table. Another bit indicates whether the entry is valid (i.e., in use) or not.<br></span>
    </div>
</details>

<details>
    <summary>9. Do que se trata o problema de page replacement?</summary>
    <br><p>O problema de page replacement consiste em escolher qual página remover da memória quando ocorre um page fault e não há espaço livre. O objetivo é selecionar uma página pouco utilizada para evitar que ela precise ser carregada novamente em breve. Se a página foi modificada, ela deve ser salva no disco antes da remoção, caso contrário, pode ser descartada.</p>
    <div style="background-color: #EEE;">
        <span>Referência na página 237 (ou 208) do livro Modern Operating Systems 5ª edição por Tanenbaum e Bos</span><br>
        <span style="color: #888;font-size: 0.8em;">When a page fault occurs, the operating system has to choose a page to evict (remove from memory) to make room for the incoming page. If the page to be removed has been modified while in memory, it must be rewritten to nonvolatile storage to bring the disk or SSD copy up to date. If, however, the page has not been changed, for example because it contains the executable code for a program, text), the disk or SSD copy is already up to date, so no rewrite is needed. The page to be read in just overwrites the page being evicted.<br>

        While it would be possible to pick a random page to evict at each page fault, system performance is much better if a page that is not heavily used is chosen. If a heavily used page is removed, it will probably have to be brought back in quickly, resulting in extra overhead. Much work has been done on the subject of page replacement algorithms, both theoretical and experimental. Below we will describe some of the most important ones.</span>
    </div>
</details>

<details>
    <summary>10. O que é o algoritmo Not Recently Used</summary>
    <br><p>O algoritmo NRU (Not Recently Used) é dos algoritmos mais famosos de page replacement. Ele remove uma página aleatória da classe de menor prioridade, baseada em bits de referência e modificação. Ele prioriza a remoção de páginas modificadas mas não referenciadas recentemente em vez de páginas limpas muito usadas. É simples de implementar e oferece desempenho razoável, embora não seja ótimo. Veja também Funcionamento de NRU, Least Recently Used (LRU) e Not Frequently Used (NFU).</p>
    <div style="background-color: #EEE;">
        <span>Referência na página 239 (ou 210) do livro Modern Operating Systems 5ª edição por Tanenbaum e Bos</span><br>
        <span style="color: #888;font-size: 0.8em;">The NRU (Not Recently Used) algorithm removes a page at random from the lowest-numbered nonempty class. Implicit in this algorithm is the idea that it is better to remove a modified page that has not been referenced in at least one clock tick (typically about 20 msec) than a clean page that is in heavy use. The main attraction of NRU is that it is easy to understand, moderately efficient to implement, and gives a performance that, while certainly not optimal, may be adequate.</span>
    </div>
</details>

Veja também Segmentação de Memória.

<!-- P3.3 – O que é a hierarquia de memóriaPonto de partida de tudo -->
<!-- P3.13 – O que é espaço de endereçamentoAbstração fundamental do SO -->
<!-- P3.20 – O que é swappingSolução clássica de memória insuficiente -->
<!-- P3.25 – Gerenciamento de memória livreComo o SO controla o que está disponível -->
<!-- P3.38 – O que é memória virtualConceito mais importante do capítulo -->
<!-- P3.40 – O que é pagingMecanismo dominante de memória virtual -->
<!-- P3.46 – O que é um page faultEvento central do funcionamento do paging -->
<!-- P3.62 – O que é TLBSolução ao gargalo de desempenho do paging -->
<!-- P3.79 – O problema de page replacementDefine a política de uso da memória física -->
<!-- P3.85 – O algoritmo NRUAlgoritmo prático mais representativo -->