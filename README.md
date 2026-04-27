# Entrada-e-Saida-Mapeada-em-Memoria-no-Processador-RISC-V

lw x1,0(x0)      # carrega de memória o endereço dos botões para x1
lw x2,4(x0)      # carrega de memória o endereço dos LEDs para x2
lw x7,8(x0)      # carrega o valor 1 (usado para incrementar/decrementar)

lw x9,12(x0)     # carrega máscara do botão 1
lw x10,16(x0)    # carrega máscara do botão 2
lw x11,20(x0)    # carrega máscara do botão 3

add x3,x0,x0     # zera o contador (x3 = 0)
sw x3,0(x2)      # escreve o valor do contador nos LEDs

lw x4,0(x1)      # lê o valor atual dos botões

and x6,x4,x9     # aplica máscara: verifica se botão 1 está pressionado
beq x6,x0,24     # se não estiver pressionado (resultado 0), pula para frente

and x6,x4,x10    # aplica máscara: verifica se botão 2 está pressionado
beq x6,x0,44     # se não estiver pressionado, pula

and x6,x4,x11    # aplica máscara: verifica se botão 3 está pressionado
beq x6,x0,64     # se não estiver pressionado, pula

beq x0,x0,-28    # salto incondicional (loop)

add x3,x3,x7     # botão 1 pressionado => incrementa contador
sw x3,0(x2)      # atualiza valor nos LEDs

lw x4,0(x1)      # lê novamente os botões
and x6,x4,x9     # aplica máscara do botão 1
sub x6,x6,x9     # verifica se botão foi solto
beq x6,x0,-52    # se soltou, volta no código

beq x0,x0,-16    # loop

sub x3,x3,x7     # botão 2 pressionado => decrementa contador
sw x3,0(x2)      # atualiza LEDs

lw x4,0(x1)      # lê botões novamente
and x6,x4,x10    # aplica máscara do botão 2
sub x6,x6,x10    # verifica se botão foi solto
beq x6,x0,-80    # se soltou, volta

beq x0,x0,-16    # loop

add x3,x0,x0     # botão 3 pressionado => zera contador
sw x3,0(x2)      # atualiza LEDs com 0

lw x4,0(x1)      # lê botões novamente
and x6,x4,x11    # aplica máscara do botão 3
sub x6,x6,x11    # verifica se botão foi solto
beq x6,x0,-108   # se soltou, volta

beq x0,x0,-16    # loop infinito
