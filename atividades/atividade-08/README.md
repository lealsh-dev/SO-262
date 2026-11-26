
# Virtualização, Hipervisores e Avaliação de Desempenho no Linux

Atividade prática da disciplina de **Sistemas Operacionais** (Curso de Engenharia de Computação — **IFCE**, Fortaleza – CE, 2026), baseada no **Capítulo 7 (*Virtualização e a Nuvem*)** de Tanenbaum & Bos.

## Sobre o Projeto

Este repositório reúne o relatório técnico e a apresentação de experimentos práticos voltados à avaliação de mecanismos modernos de virtualização em Linux. Para contrastar o impacto da virtualização aninhada frente à execução direta no hardware, todas as tarefas foram executadas em **dois ambientes distintos**:

- **Ambiente 1 (Virtualização Aninhada):** Windows 11 → Oracle VirtualBox → Ubuntu 22.04.5 → QEMU/KVM e libvirt → Alpine Linux 3.20 (processador Intel com **VT-x**).
- **Ambiente 2 (Nativo / *Bare-Metal*):** Fedora Linux 44 direto no hardware → QEMU/KVM e libvirt → Fedora Server 43 (processador AMD com **AMD-V**).

## O que foi realizado

1. **Suporte de Hardware à Virtualização (Intel VT-x / AMD-V):** validação das extensões de virtualização da CPU e resolução de conflitos de camadas no Windows 11 (Hyper-V, HVCI, VBS e Credential Guard) para habilitar virtualização aninhada no Oracle VirtualBox.
2. **Hipervisor Tipo 1 (KVM) vs. Emulação por Software (QEMU TCG):** benchmark de CPU com `sysbench` comparando ambiente de referência, VM acelerada por hardware (KVM) e emulação pura por software (TCG).
3. **Gerenciamento Dinâmico de Memória (*Memory Ballooning*):** redução a quente da memória RAM alocada à VM (de 2 GB para 1 GB) sem reinicialização, utilizando `libvirt` e o driver `virtio_balloon`.
4. **Paravirtualização de E/S (VirtIO):** inspeção dos dispositivos VirtIO e comparação de leitura sequencial entre discos paravirtualizados (`virtio-blk`) e controladores emulados (IDE no Ambiente 1 e SATA com leitura direta sem *Page Cache* no Ambiente 2).
5. **Virtualização de SO vs. Hardware (Docker vs. KVM):** comparação de tempo de inicialização, consumo de memória RAM em repouso e modelo de isolamento (*namespaces* e *cgroups v2* vs. kernel independente).

## Resumo dos Principais Resultados

| Tarefa | Ambiente 1 (Intel / Aninhado) | Ambiente 2 (AMD / *Bare-Metal*) |
| :--- | :--- | :--- |
| **1. Hardware** | Intel VT-x (`vmx`, 4 vCPUs) | AMD-V (`svm`, 12 threads) |
| **2. CPU (`sysbench`)** | KVM: **93,3%** da ref. \| TCG: **4,3×** mais lento | KVM: **99,4%** do nativo \| TCG: **6,75×** mais lento |
| **3. Memória (Balão)** | 2 GB → 1 GB (Alpine: 1980 MB → 956 MB) | 2 GB → 1 GB (Fedora: 1953 MB → 929 MB) |
| **4. Disco (VirtIO)** | 1,1 GB/s vs. 735 MB/s IDE (**1,5×** mais rápido) | 9,3 GB/s vs. 5,0 GB/s SATA (**1,86×** mais rápido) |
| **5. Docker vs. VM** | Docker: 0,59 s / 1,59 MiB (**27×** mais rápido que VM) | Docker: 0,66 s / 1,62 MiB (**20×** mais rápido que VM) |

## Equipe

- João Eduardo Viana da Silva
- João Guilherme dos Santos Leão
- Matheus Levi Fava do Carmo Pessoa
- Sérgio Henrique Lobato Leal

**Professor:** Ricardo Duarte Taveira  
**Referência principal:** TANENBAUM, A. S.; BOS, H. *Sistemas Operacionais Modernos*. 4. ed. Pearson, 2016 (Capítulo 7).
