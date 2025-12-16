<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Triangulação de Delaunay 2D e Morfologia Computaciona</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      line-height: 1.6;
      margin: 0;
      padding: 0;
      background: #ffffff;
    }

    header {
      background: #4a148c;
      color: white;
      padding: 30px 20px;
      text-align: center;
    }

    header h1 {
      margin: 0;
      font-size: 32px;
    }

    nav {
      text-align: center;
      margin: 25px 0;
    }

    nav a {
      background: #6a1b9a;
      color: white;
      padding: 12px 24px;
      border-radius: 6px;
      text-decoration: none;
      font-weight: bold;
      transition: background 0.2s ease;
    }

    nav a:hover {
      background: #4a148c;
    }

    main {
      max-width: 900px;
      margin: 0 auto 40px auto;
      background: white;
      padding: 30px;
      border-radius: 8px;
    }

    h2 {
      color: #4a148c;
      margin-top: 40px;
    }

    section {
      margin-bottom: 40px;
    }

    img {
      width: 100%;
      max-width: 500px;
      display: block;
      margin: 20px auto;
    }

    p {
      text-align: justify;
    }
  </style>
</head>
<body>

  <header>
    <h1>Formação de Malhas</h1>
    <p>Triangulações de Delaunay</p>
  </header>

  <nav>
    <a href="https://github.com/joaobalieiro/TriangulacaoDelaunay.git" target="_blank">GitHub do Trabalho</a>
  </nav>

  <main>

    <section>
      <h2>Introdução</h2>
      <p>
        Há diversas formas de triangularizar um conjunto de pontos. Dentre elas, a triangulação de Delaunay
        tem destaque na geração de malhas para computação gráfica, pois ela maximiza o menor ângulo dos
        triângulos que a compõem. Como motivação, considere triangulações de um conjunto de pontos de um
        determinado relevo, onde os pontos centrais representam os pontos mais altos.
      </p>
      <img src="im2.png" alt="Fonte: https://ti.inf.ethz.ch/ew/courses/Geo22/lecture/gca22">

      <!-- INSERIR IMAGEM AQUI: comparação entre triangulações -->

      <p>
        Ambas são triangulações válidas. No entanto, uma delas aparenta ser mais natural devido ao seu
        triângulo central não estar tão achatado quanto o da outra. Esse achatamento está diretamente
        relacionado ao menor ângulo do triângulo, sendo possível comparar a qualidade entre essas
        triangulações a partir desse ângulo.
      </p>
    </section>

    <section>
      <h2>Delaunay: a triangulação ideal</h2>
      <p>
        Considere Υ(T) = (β1, β2, β3, ..., β3t) o vetor dos ângulos de uma triangulação T em
        ordem crescente. Uma triangulação T1 é dita melhor que T2 se Υ(T1) > Υ(T2), segundo a ordem
        lexicográfica. Ao comparar duas triangulações, avalia-se inicialmente o menor ângulo; caso eles
        sejam iguais, passa-se ao segundo menor, e assim sucessivamente.
      </p>

      <!-- INSERIR IMAGEM AQUI: losango azul e losango verde -->
      <img src="im3.png" alt=" Fonte: https://www.youtube.com/watch?v=np4Q9NiYMZU ">

      <p>
        Partindo de uma triangulação não ideal, é possível chegar a uma versão melhorada através de flips
        de arestas, de forma que o menor ângulo dos novos triângulos seja maximizado. Arestas que aumentam
        os ângulos mínimos ao serem flipadas são chamadas de arestas ilegais e podem ser identificadas
        utilizando o Teorema de Tales. Através de flips sucessivos dessas arestas ilegais, obtém-se a
        triangulação ideal, conhecida como triangulação de Delaunay.
      </p>

      <p>
        A triangulação de Delaunay é considerada ideal pois maximiza o menor ângulo dos triângulos,
        minimiza o maior circuncírculo e, consequentemente, produz triângulos menos achatados. Por
        definição, um triângulo é chamado de Delaunay se seus vértices pertencem a um conjunto finito de
        pontos P e se o seu circuncírculo é vazio, isto é, não contém nenhum outro ponto de P em seu
        interior.
      </p>

      <!-- INSERIR IMAGEM AQUI: circuncírculo vazio -->
      <img src="im4.png" alt=" http://www.geom.uiuc.edu/~samuelp/del_project.html">

    </section>

    <section>
      <h2>Malha de Delaunay</h2>
      <p>
        Malhas de Delaunay são excelentes opções quando se deseja evitar triângulos com ângulos muito
        pequenos. Existem diversos métodos para a obtenção dessas malhas; neste trabalho, serão abordados
        os métodos de Flipping e Parabolic Lifting.
      </p>
    </section>

    <section>
      <h2>Parabolic Lifting</h2>
      <p>
        Considere um conjunto P de pontos no plano e um paraboloide acima desse plano. O método de
        Parabolic Lifting consiste em elevar cada ponto do plano para o espaço tridimensional. Um ponto
        (x, y) ∈ R² é transformado em (x, y, x² + y²) ∈ R³, posicionando todos os pontos sobre um
        paraboloide convexo.
      </p>

      <!-- INSERIR IMAGEM AQUI: pontos elevados ao paraboloide -->
      <img src="im5.png" alt="https://www.youtube.com/watch?v=NcAKzVoFbVI">


      <p>
        Em seguida, constrói-se o fecho convexo dos pontos elevados sobre o paraboloide. A projeção do
        casco convexo inferior de volta ao plano resulta em uma triangulação de Delaunay.
      </p>

      <!-- INSERIR IMAGEM AQUI: fecho convexo e projeção -->
      <img src="im6.png" alt="fonte:https://www.youtube.com/watch?v=NcAKzVoFbVI">

    </section>

    <section>
      <h2>Flipping</h2>
      <p>
        O método de Flipping é de fácil visualização e bastante intuitivo. Inicialmente, constrói-se um
        grande triângulo que contenha todos os pontos do conjunto. Em seguida, escolhe-se um ponto e
        conecta-se esse ponto aos vértices do triângulo no qual ele está inserido, subdividindo-o.
      </p>

      <p>
        Esse processo é repetido para os pontos restantes. Sempre que surgirem arestas ilegais, realizam-se
        flips até que todas sejam eliminadas. Caso um ponto esteja localizado exatamente sobre uma
        aresta, essa aresta é removida e o ponto é conectado aos quatro vértices acessíveis, formando novos
        triângulos.
      </p>

      <p>
        Ao final do processo, remove-se o triângulo inicial que envolvia todos os pontos. A malha obtida
        é uma malha de Delaunay.
      </p>

      <!-- INSERIR IMAGEM AQUI: passos do flipping -->
      <img src="im7.png" alt="Construa o triângulo FGH que envolva todos os pontos.">
      <img src="im8.png" alt="    2. Tome um ponto qualquer e ligue-o a cada um dos vértices do triângulo, dividindo-o em três. Construa o triângulo FGH que envolva to
      ">
      <img src="im9.png=" alt="">
      <img src="im10.png" alt="Repita o processo para os pontos restantes">
      

    </section>

    <section>
      <h2>Aplicação</h2>
      <p>
        Triângulos pouco achatados tornam as malhas de Delaunay extremamente úteis em problemas de
        interpolação. Considere novamente duas triangulações de um relevo. Suponha que a altura de um
        ponto intermediário seja desconhecida.
      </p>

      <!-- INSERIR IMAGEM AQUI: relevo e ponto q -->
      <img src="im12.png" alt="Fonte: Berg, M. Computational Geometry: Algorithms and Applications">


      <p>
        Ao realizar a interpolação pela média dos pontos conectados, obtêm-se valores muito distintos a
        depender da triangulação utilizada. A triangulação que maximiza o menor ângulo tende a conectar
        o ponto a vizinhos mais próximos, produzindo resultados mais realistas e coerentes com a
        geometria do relevo.
      </p>
    </section>

    <section>
      <h2>Malhas de Delaunay Conformadas (CDT)</h2>
      <p>
        A construção de uma CDT começa com a geração de uma triangulação de Delaunay sobre o
conjunto de pontos PPP. Em seguida, cada segmento restrito s ∈ S é imposto à malha: se ele
não estiver presente, identifica-se os triângulos que o intersectam e inserem-se pontos Steiner
ao longo do segmento até que ele possa ser representado como uma aresta da triangulação.
Após incorporar todos os segmentos, aplica-se um refinamento de qualidade (como os
métodos de Ruppert ou Chew) para remover triângulos pobres e melhorar a distribuição dos
elementos. O resultado é uma triangulação que respeita o contorno e garante boa qualidade
geométrica.
      </p>
      <img src="img13.png" alt="">

      <p>
        A construção de uma CDT começa com a geração de uma triangulação de Delaunay sobre um conjunto de
        pontos. Em seguida, cada segmento restrito é imposto à malha. Caso ele não esteja presente,
        inserem-se pontos de Steiner ao longo do segmento até que ele possa ser representado como uma
        aresta da triangulação. Após isso, aplica-se um refinamento de qualidade, como os métodos de
        Ruppert ou Chew.
      </p>
    </section>

    <section>
      <h2>Implementações do Código</h2>
      <p>
        <b>parabolic_lifting:</b> parabolic_lifting: Este código aplica a técnica de elevar pontos 2D para 3D sobre um
        paraboloide. Dado um ponto (x,y) do plano ele é transformado em um ponto tridimensional:
        de forma:(x,y,x +y2 ). Essa transformação vai colocar todos os pontos sobre um paraboloide2
        convexo. Ao elevar pontos nessa superfície faz com que a triangulação de Delauney no plano
        corresponda exatamente às faces da casca convexa inferior. Assim, as propriedades
        complexas difíceis de se verificar em 2D tornam-se mais simples na convexidade 3D.
      </p>

      <!-- INSERIR IMAGEM AQUI: código parabolic lifting -->
      <img src="im15.jpeg" alt="Fonte: Elaborado pelo autor">


      <p>
        <b>delaunay_triangulation:</b>delauney_triangulation: Essa função implementa a técnica para construir a trianuglação 2D
        para um conjunto de pontos, explorando a propriedade fundamental que um conjunto de
        pontos no plano é equivalente a projeção do Lower Hulln (casco inferior) do casco convexo
        dos pontos após serem elevados a R3 por um mapeamento parabólico (a função parabolic
        lifting). Dessa forma, o método transforma o problema de otimização geométrixa 2D em um
        problema convexo 3D.
        Portanto, a função parabolic_lifting irá realizar o mapeamento parabólica, enquanto a função
        delaunay_triangulation aplica o casco convexo e extrai os triângulos válidos
      </p>

      <!-- INSERIR IMAGEM AQUI: triangulação em 3D -->]
      <img src="im14.jpeg" alt="Fonte: Elaborado pelo autor">


    </section>

  

    <section>
      <h2>O que é CDT?</h2>
      <p>
        A Triangulação de Delaunay com Restrição, ou CDT, é uma extensão da triangulação de Delaunay
        desenvolvida para lidar com domínios que possuem contornos, interfaces internas, furos ou qualquer
        estrutura geométrica que deve ser preservada exatamente. Enquanto a triangulação de Delaunay
        comum busca apenas otimizar propriedades geométricas, como evitar triângulos muito agudos, a CDT
        adapta essa triangulação para incorporar obrigatoriamente as fronteiras definidas pelo problema.
      </p>
      <p>
        Sua origem está na necessidade de aplicar Delaunay em situações reais, onde a geometria não é
        simplesmente um conjunto de pontos, mas sim um domínio com limites bem definidos. No contexto da
        morfologia computacional, a CDT é especialmente útil porque permite representar regiões complexas
        antes de aplicar operações morfológicas, garantindo que a análise de forma, a segmentação ou a
        reconstrução estrutural ocorram de modo consistente com a geometria verdadeira do domínio.
      </p>
    </section>

    <section>
      <h2>O que é PLC?</h2>
      <p>
        O Piecewise Linear Complex, ou PLC, é a estrutura que descreve com precisão a geometria do domínio
        onde a triangulação será feita. Ele é formado por vértices e segmentos que representam bordas,
        interfaces internas, furos e qualquer elemento que precisa ser preservado. Em essência, o PLC
        reúne todas as restrições que a CDT deve obrigatoriamente incorporar.
      </p>
      <p>
        O PLC também introduz o conceito de visibilidade, essencial quando o domínio possui fronteiras e
        obstáculos internos. Dois pontos são considerados visíveis se o segmento que os conecta está
        inteiramente contido no domínio e não é interceptado por nenhum segmento do PLC. Essa noção é
        fundamental para adaptar o critério clássico do círculo vazio às CDTs.
      </p>

      <!-- INSERIR IMAGEM AQUI: PLC e CDT correspondente -->
      <img src="im16.jpeg" alt="fonte: delnotes">

    </section>

    <section>
      <h2>Implementação da Recuperação de Restrições</h2>
      <p>
        Quando um segmento do contorno precisa ser incorporado à triangulação, o passo essencial é
        eliminar todas as arestas que o interceptam. Cada aresta que cruza um segmento do PLC viola a
        CDT e precisa ser removida. Para isso, o algoritmo utiliza o flip de diagonal.
      </p>
      <img src="im17.jpeg" alt="fonte:delnotes">

      <p>
        Ao identificar uma aresta que cruza o segmento restrito, analisam-se os dois triângulos
        adjacentes, que formam um quadrilátero convexo. A diagonal que causa a interseção é substituída
        pela outra possível, gerando dois novos triângulos que não cruzam mais o segmento do PLC.
      </p>
      <p>
        Esse processo é repetido até que o segmento restrito esteja completamente presente na malha.
        Em seguida, as novas arestas são verificadas para restaurar a condição de Delaunay sempre que
        possível dentro das restrições impostas.
        <img src="im18.jpeg" alt="fonte:delnotes">

      </p>
    </section>

    <section>
      <h2>Refinamento de Triangulações de Delaunay em 2D</h2>
      <p>
        Depois de construir uma triangulação de Delaunay para o conjunto de pontos do “lago”, o
código aplica dois esquemas de refinamento de malha: um no estilo de Chew e outro no
estilo de Ruppert. O objetivo em ambos os casos é melhorar a qualidade dos triângulos,
controlando ângulos muito agudos e respeitando a fronteira do polígono que representa o
lago.
A triangulação de Delaunay em si é obtida pela função delaunay_triangulation, que
usa o parabolic lifting e o casco convexo em (R^3). Sobre esta triangulação, as funções
chew_refinement e ruppert_refinement iterativamente inserem novos pontos,
recalculando a triangulação a cada passo.
      </p>
    </section>

    <section>
      <h2>Critério de Qualidade</h2>
      <p>
        Na implementação, o critério de qualidade é baseado no ângulo mínimo de cada triângulo.
Esse cálculo é feito pela função triangle_geom(pts, tri), que recebe o vetor de
pontos pts e um triângulo tri (índices dos vértices) e devolve três informações: a área do triângulo; o menor ângulo interno (em graus);o comprimento da maior aresta.
      </p>
      <p>Para calcular os ângulos, a função usa a lei dos cossenos. Dados os comprimentos das
        arestas (a), (b) e (c), o ângulo oposto ao lado (a), por exemplo, é obtido por
       <p> cos(theta_a) = {b^2 + c^2 - a^2}/{2bc},</p>
       <p> e depois (theta_a = arccos(cos(theta_a))), convertido para graus. O menor entre os três
        ângulos é tomado como medida de qualidade: quanto menor esse valor, mais fino e pior é o
        triângulo.</p>
        <p>A função get_bad_triangles(pts, tris, min_angle_deg, poly, max_bad)
            percorre todos os triângulos tris e</p>
            <p>1.Calcula o centroide do triângulo e verifica se ele está dentro do polígono do lago
                usando point_in_polygon. Triângulos fora do domínio são ignorados.</p>
                <p>2. Usa triangle_geom para obter a área e o ângulo mínimo</p>
                <p>3. Descarta triângulos quase degenerados (área muito pequena).</p>
                <p>4. Marca como “ruins” aqueles cujo ângulo mínimo é menor que o limiar
                    min_angle_deg (por exemplo, 28° ou 30°).</p>
                    <p>A função devolve a lista de índices desses triângulos ruins, ordenados pelo ângulo mínimo
                        (do pior para o menos ruim) e, opcionalmente, limitada por max_bad para não refinar tudo
                        de uma vez</p>
    </section>
<section>
    <h2>Cálculo do circuncentro e pontos de refinamento</h2>
    <p>Quando um triângulo é identificado como ruim, a posição natural para inserir um novo
        vértice é o seu circuncentro. Isso é feito pela função triangle_circumcenter(pts,
        tri):</p>
        <p>● primeiro, ela chama circumcircle(p1, p2, p3), que recebe as coordenadas
            dos três vértices e resolve analiticamente o sistema das mediatrizes, obtendo:</p>
            <p>○ o centro ((u_x, u_y)) do circuncírculo;</p>
            <p>○ o raio ao quadrado (r^2);</p>
            <p>● se o triângulo for degenerado (determinante próximo de zero), a função retorna o
                baricentro como fallback;</p>
                <p>● caso contrário, devolve o ponto ((c_x, c_y)) correspondente ao circuncentro</p>
                <p>Esse ponto é, em princípio, o candidato a ser inserido na malha para eliminar o triângulo
                    skinny. No entanto, antes de inseri-lo, o código verifica se o circuncentro está dentro do lago
                    (point_in_polygon(cc, poly)); circuncentros fora do domínio são descartados.</p>
        </section>
    <section>
      <h2>Refinamento tipo Chew</h2>
      <p>
        A função chew_refinement(points, segments, poly, min_angle_deg,
iterations, max_new_each) implementa um refinamento simplificado no estilo de
Chew. A lógica é:</p>
<p>Recebe:
   <p> ○ points: conjunto inicial de pontos (fronteira + pontos internos);</p>
    <p>○ segments: lista de arestas que representam a fronteira do lago;</p>
    <p>○ poly: polígono do lago (para o teste ponto-no-polígono);</p>
    <p>○ parâmetros de qualidade: min_angle_deg (ângulo mínimo desejado),
    iterations (número máximo de ciclos) e max_new_each (limite de novos
    pontos por iteração).</p>
    <p>Recalcula a triangulação de Delaunay com
        delaunay_triangulation(pts).</p>
        <p>
        ○ Usa get_bad_triangles para identificar triângulos com ângulo mínimo
        abaixo de min_angle_deg, limitando o número de triângulos examinados a
        max_new_each.</p>
        <p>
        ○ Se não há novos pontos a inserir, o processo é interrompido.
        ○ Caso contrário, os novos pontos são empilhados em pts via np.vstack, e
        passa-se para a próxima iteração.</p>
       <p> 4. Ao terminar todas as iterações (ou quando não há mais triângulos ruins), o código
        faz uma última chamada a delaunay_triangulation e
        recover_constraints, resultando em uma malha refinada. A função retorna pts
        (pontos finais) e tris (triângulos finais).</p>
        <p>Nesse esquema, o refinamento atua exclusivamente inserindo circuncentros de triângulos
            ruins. As fronteiras são preservadas pela rotina de recuperação de restrições, mas não há
            ainda o tratamento explícito de “segmentos invadidos” típico de Rupper</p>
        <img src="im19.png" alt="">

      </p>
    </section>

    <section>
      <h2></h2>Encroachment de segmentos na implementação </h2>
      <p>
        Para lidar com fronteiras de forma mais sofisticada, o código introduz o conceito de
segmento encroached na função find_encroached_segment(candidate_point,
pts, segments). A ideia é aproximar o critério do círculo diametral:
      </p>
      <p>● para cada segmento ((a, b)) da lista segments, calcula-se:</p>
      ,<p>○ o ponto médio mid = 0.5 * (pa + pb);</p>
      <p>○ o raio ao quadrado do círculo diametral, r2 = ||pa - mid||² (metade do
        comprimento do segmento ao quadrado);</p>
        <p>○ a distância ao quadrado do candidato ao ponto médio, d2 =
            ||candidate_point - mid||².</p>
            <p>● se d2  r2 - 1e-10, interpreta-se que o ponto candidato está dentro do círculo
                diametral do segmento ((a, b)); isso caracteriza o segmento como encroached e a
                função devolve esse segmento.</p>
                <p>Se nenhum segmento for invadido, a função retorna None. Esse teste é usado para decidir
                    se o refinamento deve inserir um circuncentro diretamente ou, antes disso, subdividir algum
                    segmento de fronteira.</p>
                    

    
</section>
<section>
    <h2>Refinamento tipo Ruppert</h2>
    <p>A função ruppert_refinement(points, segments, poly, min_angle_deg,
        iterations, max_new_each) usa a mesma base que chew_refinement, mas
        incorpora o tratamento de segmentos encroached, aproximando o algoritmo clássico de
        Ruppert:</p>
        <p>○ pts começa com os pontos iniciais (fronteira + pontos internos);</p>
        <p>○ segs é a lista de segmentos de fronteira do lago</p>
        <p>○ Calcula a triangulação de Delaunay com
            delaunay_triangulation(pts).</p>
            <p>○ Recupera as restrições com recover_constraints(pts, tris,
                segs).</p>
                    <p>○ Identifica triângulos ruins com get_bad_triangles, como antes.</p>
                    <p>Para cada triângulo ruim:</p>
                    <p>○ Calcula o circuncentro cc</p>
                    <p>○ Descarta circuncentros fora do lago (point_in_polygon(cc, poly)).</p>
                    <p>○ Chama find_encroached_segment(cc, pts, segs)</p>
                   <p> ■ se não existir segmento encroached, o circuncentro é aceito como
                    novo ponto de refinamento (new_pts.append(cc)).</p>
                   <p>■ se existir um segmento encroached ((a, b)), o código</p>
                   <p> calcula o ponto médio mid = 0.5 * (pts[a] + pts[b])</p>
                   <p>■ verifica se o ponto médio está dentro do lago;</p>
                   <p>■ reserva um índice para o novo ponto (será len(pts) +
                    len(new_pts));</p>
                   <p>■ substitui o segmento ((a, b)) por dois subsegmentos ((a,
                    new_index)e(new_index, b)emnew_segs`;</p>
                   <p>■ adiciona o ponto médio à lista de novos pontos</p>
                   <p>4. Ao final da iteração</p>
                   <p>○ se não há novos pontos, o refinamento é encerrado;</p>
                   <p>○ caso contrário, empilha-se new_pts em pts e, se houve encroachment,
                    atualiza-se a lista de segmentos segs = new_segs.</p>
                   <p>Ao final de todas as iterações (ou na interrupção antecipada), faz-se uma última
                    triangulação com delaunay_triangulation e recover_constraints,
                    obtendo-se a malha refinada e a nova lista de segmentos. A função retorna pts,
                    tris e segs.</p>
                   <p>Em resumo, o refinamento tipo Ruppert implementado segue a lógica:</p>
                   <p>● prioridade para segmentos encroached: se o circuncentro invadir o círculo
                    diametral de algum segmento, o segmento é subdividido;</p>
                   <p>● só quando não há encroachment é que o circuncentro é de fato inserido.</p>
                   <p>Isso faz com que a fronteira do lago seja refinada onde necessário, evitando triângulos de
                    baixa qualidade junto à borda.</p>
                    
                        <h2>Geração das figuras de evolução da malha</h2>
                   <p>Por fim, a função generate_all() organiza a aplicação desses métodos e gera as
                    imagens que ilustram a evolução da malha:</p>
                   <p>1. generate_lake_polygon constrói um polígono irregular (“lago”) em ([0,1]^2).</p>
                   <p>2. sample_points_in_polygon adiciona pontos internos aleatórios.</p>
                   <p>3. A lista segments contém os segmentos de fronteira do lago, conectando os vértices
                    da borda.</p>
                   <p>4. São geradas, em sequência</p>
                   <p>○ a triangulação de Delaunay sem restrições;</p>
                   <p>○ a triangulação com restrição (CDT aproximada</p>
                   <p>○ a malha refinada por chew_refinement;</p>
                   <p>○ a malha refinada por ruppert_refinement.</p>
                   <p>Cada uma dessas malhas é desenhada por plot_triangulation, que plota apenas
                    triângulos cujo centroide está dentro do lago, desenha os segmentos de fronteira e salva o
                    resultado em arquivos PNG. Essas imagens são usadas para visualizar o efeito do
                    refinamento: redução de triângulos com ângulos muito agudos e melhor adaptação da
                    malha à geometria do domínio</p>
                    
                   <p></p>
                   <p></p>
                   <p></p>
                   <img src="im20.png" alt="">
    </section>


    <section id="autores">
        <h2>Autores</h2>
        <p>
        Trabalho desenvolvido por alunos da disciplina de Introdução à Modelagem,
        abordando conceitos de Triangulação de Delaunay, CDT, PLC e refinamento de malhas.
        </p>
        <p>
        <b>Autores:</b><br>
        • João Pedro Pereira Balieiro:elaboração dos textos, slides,Código e apresentação<br>
        • Millena Farias dos Santos: desenvolvimento dos textos e slides<br>
        • Mariana Barbosa Queroz: desenvolvimento dos textos, slides, apresentação e homepage<br>
        •Bruno Mendonça: desenvolvimento dos textos, slides e apresentação<br>
        </p>
        </section>
        
        
        <section id="fontes">
        <h2>Fontes e Referências</h2>
        <p>
        BERG, M. et al. <i>Computational Geometry: Algorithms and Applications</i>.<br>
        Lecture Notes on
        Delaunay Mesh Generation:Jonathan Richard Shewchuk
        February 5, 2012<br>
        Desenvolvimento de um gerador de malhas
Delaunay em três dimensões:Bruno Nogueira Machadobr>
        
        </p>
        </section>

  </main>

</body>
</html>
