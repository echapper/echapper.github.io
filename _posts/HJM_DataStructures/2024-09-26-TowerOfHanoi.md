<article class="px-1">
  <header>
    <h1 data-toc-skip>Tower of Hanoi, HJM Data Stuctures 4-3</h1>
    

    <div class="post-meta text-muted">
      <!-- published date -->
      <span>
        Posted
        <!--
  Date format snippet
  See: ${JS_ROOT}/utils/locale-dateime.js
-->




<time
  
  data-ts="1727350200"
  data-df="ll"
  
    data-bs-toggle="tooltip" data-bs-placement="bottom"
  
>
  Sep 26, 2024
</time>

      </span>

      <!-- lastmod date -->
      

      

      <div class="d-flex justify-content-between">
        <!-- author(s) -->
        <span>
          

          By

          <em>
            
              <a href="https://github.com/echapper">Antoniy</a>
            
          </em>
        </span>

        <div>
          <!-- pageviews -->
          

          <!-- read time -->
          <!-- Calculate the post's reading time, and display the word count in tooltip -->



<!-- words per minute -->










<!-- return element -->
<span
  class="readtime"
  data-bs-toggle="tooltip"
  data-bs-placement="bottom"
  title="3708 words"
>
  <em>20 min</em> read</span>

        </div>
      </div>
    </div>
  </header>

  <div class="content">
    <div class="notice">
홍정모 교수님의 자료구조 강의에 기반한 글입니다.<br />
🌟<a href="https://honglab.co.kr/courses/data-structures" target="_blank">들으러가기</a>
</div>

<h2 id="tower-of-hanoi"><span class="me-2">Tower of Hanoi</span><a href="#tower-of-hanoi" class="anchor text-muted"><i class="fas fa-hashtag"></i></a></h2>

<p>하노이의 탑은 3개의 기둥 중 어느 한 곳에서 크기가 가장 큰 것 부터 가장 아래로 깔려져있는 디스크들을 같은 정렬 방식으로 다른 기둥으로 옮기는 게임이다.</p>

<p>이를 재귀함수, stack를 이용하여 구현해보자.</p>

<figure style="text-align: center; zoom:50%;">
    <a href="/../../images/2024-09-26-TowerOfHanoi/image-20240926211223846.png" class="popup img-link  shimmer"><img src="/../../images/2024-09-26-TowerOfHanoi/image-20240926211223846.png" alt="image" style="max-width: 100%; height: auto;" loading="lazy"></a>
    <figcaption style="font-size: 24px; color: gray;"><a href="https://www.mathsisfun.com/games/towerofhanoi.html" target="_blank">플레이하러 가기</a>
    </figcaption>
</figure>

<h2 id="기본-소스코드"><span class="me-2">기본 소스코드</span><a href="#기본-소스코드" class="anchor text-muted"><i class="fas fa-hashtag"></i></a></h2>

<div class="language-c++ highlighter-rouge"><div class="code-header">
        <span data-label-text="C++"><i class="fas fa-code fa-fw small"></i></span>
      <button aria-label="copy" data-title-succeed="Copied!"><i class="far fa-clipboard"></i></button></div><div class="highlight"><code><table class="rouge-table"><tbody><tr><td class="rouge-gutter gl"><pre class="lineno">1
2
3
4
5
6
7
</pre></td><td class="rouge-code"><pre><span class="cp">#include</span> <span class="cpf">&lt;iostream&gt;</span><span class="cp">
</span>
<span class="cp">#include</span> <span class="cpf">&lt;share/stack.h&gt;</span><span class="c1"> //전 강의에서 구현한 stack, 없으면 std::stack이용</span><span class="cp">
</span>
<span class="k">using</span> <span class="k">namespace</span> <span class="n">std</span><span class="p">;</span>

<span class="n">Stack</span><span class="o">&lt;</span><span class="kt">char</span><span class="o">&gt;</span> <span class="n">tower</span><span class="p">[</span><span class="mi">3</span><span class="p">];</span>
</pre></td></tr></tbody></table></code></div></div>

<p>ㅤ</p>

<ul>
  <li>PrintTowers()
    <ul>
      <li>stack형인 <code class="language-plaintext highlighter-rouge">tower</code>의 내장함수 <code class="language-plaintext highlighter-rouge">Print()</code>를 이용하여 인덱스별 원소 출력</li>
    </ul>
  </li>
</ul>

<div class="language-c++ highlighter-rouge"><div class="code-header">
        <span data-label-text="C++"><i class="fas fa-code fa-fw small"></i></span>
      <button aria-label="copy" data-title-succeed="Copied!"><i class="far fa-clipboard"></i></button></div><div class="highlight"><code><table class="rouge-table"><tbody><tr><td class="rouge-gutter gl"><pre class="lineno">1
2
3
4
5
6
7
8
9
10
</pre></td><td class="rouge-code"><pre><span class="kt">void</span> <span class="nf">PrintTowers</span><span class="p">()</span>
<span class="p">{</span>
	<span class="n">cout</span> <span class="o">&lt;&lt;</span> <span class="s">"Towers"</span> <span class="o">&lt;&lt;</span> <span class="n">endl</span><span class="p">;</span>
	<span class="n">cout</span> <span class="o">&lt;&lt;</span> <span class="s">"0: "</span><span class="p">;</span>
	<span class="n">tower</span><span class="p">[</span><span class="mi">0</span><span class="p">].</span><span class="n">Print</span><span class="p">();</span>
	<span class="n">cout</span> <span class="o">&lt;&lt;</span> <span class="s">"1: "</span><span class="p">;</span>
	<span class="n">tower</span><span class="p">[</span><span class="mi">1</span><span class="p">].</span><span class="n">Print</span><span class="p">();</span>
	<span class="n">cout</span> <span class="o">&lt;&lt;</span> <span class="s">"2: "</span><span class="p">;</span>
	<span class="n">tower</span><span class="p">[</span><span class="mi">2</span><span class="p">].</span><span class="n">Print</span><span class="p">();</span>
<span class="p">}</span>
</pre></td></tr></tbody></table></code></div></div>

<p>ㅤ</p>

<ul>
  <li>MoveDisk()
    <ul>
      <li>각 기둥<span class="faded_italic-text"> tower[n] </span>에서, <code class="language-plaintext highlighter-rouge">from</code>의 가장 위에 있는 요소를 <code class="language-plaintext highlighter-rouge">to</code>로 이동
        <ul>
          <li><code class="language-plaintext highlighter-rouge">to</code>가 비어있는 stack이면 X</li>
          <li><code class="language-plaintext highlighter-rouge">tower[to].Top() &gt; disk</code>시 오류이므로, 목적지의 <code class="language-plaintext highlighter-rouge">Top</code>이 가져오는 <code class="language-plaintext highlighter-rouge">Top</code>보다 작아야한다.
=&gt; A B C … 순서로 쌓인다<span class="faded_italic-text dimmed-text">(ascii)</span></li>
        </ul>
      </li>
    </ul>
  </li>
</ul>

<div class="language-c++ highlighter-rouge"><div class="code-header">
        <span data-label-text="C++"><i class="fas fa-code fa-fw small"></i></span>
      <button aria-label="copy" data-title-succeed="Copied!"><i class="far fa-clipboard"></i></button></div><div class="highlight"><code><table class="rouge-table"><tbody><tr><td class="rouge-gutter gl"><pre class="lineno">1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
</pre></td><td class="rouge-code"><pre><span class="kt">void</span> <span class="nf">MoveDisk</span><span class="p">(</span><span class="kt">int</span> <span class="n">from</span><span class="p">,</span> <span class="kt">int</span> <span class="n">to</span><span class="p">)</span>
<span class="p">{</span>
	<span class="k">if</span> <span class="p">(</span><span class="n">tower</span><span class="p">[</span><span class="n">from</span><span class="p">].</span><span class="n">IsEmpty</span><span class="p">())</span>
	<span class="p">{</span>
		<span class="n">cout</span> <span class="o">&lt;&lt;</span> <span class="s">"Tower "</span> <span class="o">&lt;&lt;</span> <span class="n">from</span> <span class="o">&lt;&lt;</span> <span class="s">"is empty."</span> <span class="o">&lt;&lt;</span> <span class="n">endl</span><span class="p">;</span>
		<span class="n">exit</span><span class="p">(</span><span class="mi">0</span><span class="p">);</span> <span class="c1">// 오류 강제 종료</span>
	<span class="p">}</span>

	<span class="k">auto</span> <span class="n">disk</span> <span class="o">=</span> <span class="n">tower</span><span class="p">[</span><span class="n">from</span><span class="p">].</span><span class="n">Top</span><span class="p">();</span>

	<span class="c1">//받을 타워가 비어 있으면 뭐든지 받을 수 있음</span>
	<span class="c1">//알파벳 순서여야 받을 수 있음 (역순 X)</span>
	<span class="k">if</span> <span class="p">(</span><span class="o">!</span><span class="n">tower</span><span class="p">[</span><span class="n">to</span><span class="p">].</span><span class="n">IsEmpty</span><span class="p">()</span> <span class="o">&amp;&amp;</span> <span class="n">tower</span><span class="p">[</span><span class="n">to</span><span class="p">].</span><span class="n">Top</span><span class="p">()</span> <span class="o">&gt;</span> <span class="n">disk</span><span class="p">)</span>
	<span class="p">{</span>
		<span class="n">cout</span> <span class="o">&lt;&lt;</span> <span class="s">"Cannot place "</span> <span class="o">&lt;&lt;</span> <span class="n">disk</span> <span class="o">&lt;&lt;</span> <span class="s">" on "</span> <span class="o">&lt;&lt;</span> <span class="n">tower</span><span class="p">[</span><span class="n">to</span><span class="p">].</span><span class="n">Top</span><span class="p">()</span> <span class="o">&lt;&lt;</span> <span class="n">endl</span><span class="p">;</span>
		<span class="n">exit</span><span class="p">(</span><span class="mi">0</span><span class="p">);</span> <span class="c1">// 오류 강제 종료</span>
	<span class="p">}</span>

	<span class="n">tower</span><span class="p">[</span><span class="n">from</span><span class="p">].</span><span class="n">Pop</span><span class="p">();</span>
	<span class="n">tower</span><span class="p">[</span><span class="n">to</span><span class="p">].</span><span class="n">Push</span><span class="p">(</span><span class="n">disk</span><span class="p">);</span>

	<span class="n">cout</span> <span class="o">&lt;&lt;</span> <span class="s">"Move disk "</span> <span class="o">&lt;&lt;</span> <span class="n">disk</span> <span class="o">&lt;&lt;</span> <span class="s">" from "</span> <span class="o">&lt;&lt;</span> <span class="n">from</span> <span class="o">&lt;&lt;</span> <span class="s">" to "</span> <span class="o">&lt;&lt;</span> <span class="n">to</span> <span class="o">&lt;&lt;</span> <span class="n">endl</span><span class="p">;</span>
	<span class="n">PrintTowers</span><span class="p">();</span>
<span class="p">}</span>
</pre></td></tr></tbody></table></code></div></div>

<p>ㅤ</p>

<ul>
  <li><code class="language-plaintext highlighter-rouge">num_disks</code>는 스택(기둥)에 쌓을 초기 요소 개수
    <ul>
      <li>ex) A, B, C</li>
      <li><code class="language-plaintext highlighter-rouge">RecurMoveDisks(num_disks, from, temp, to)</code> =&gt; 구현해야하는 디스크 이동정렬 함수</li>
    </ul>
  </li>
</ul>

<div class="language-c++ highlighter-rouge"><div class="code-header">
        <span data-label-text="C++"><i class="fas fa-code fa-fw small"></i></span>
      <button aria-label="copy" data-title-succeed="Copied!"><i class="far fa-clipboard"></i></button></div><div class="highlight"><code><table class="rouge-table"><tbody><tr><td class="rouge-gutter gl"><pre class="lineno">1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
</pre></td><td class="rouge-code"><pre><span class="kt">int</span> <span class="nf">main</span><span class="p">()</span>
<span class="p">{</span>
	<span class="kt">int</span> <span class="n">num_disks</span> <span class="o">=</span> <span class="mi">3</span><span class="p">;</span>

	<span class="k">for</span> <span class="p">(</span><span class="kt">int</span> <span class="n">i</span> <span class="o">=</span> <span class="mi">0</span><span class="p">;</span> <span class="n">i</span> <span class="o">&lt;</span> <span class="n">num_disks</span><span class="p">;</span> <span class="n">i</span><span class="o">++</span><span class="p">)</span>
		<span class="n">tower</span><span class="p">[</span><span class="mi">0</span><span class="p">].</span><span class="n">Push</span><span class="p">(</span><span class="sc">'A'</span> <span class="o">+</span> <span class="n">i</span><span class="p">);</span>

	<span class="n">PrintTowers</span><span class="p">();</span>

	<span class="n">RecurMoveDisks</span><span class="p">(</span><span class="n">num_disks</span><span class="p">,</span> <span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">2</span><span class="p">);</span>

	<span class="n">PrintTowers</span><span class="p">();</span>

	<span class="k">return</span> <span class="mi">0</span><span class="p">;</span>
<span class="p">}</span>
</pre></td></tr></tbody></table></code></div></div>

<p>ㅤ</p>

<ul>
  <li>구현부 ↴</li>
</ul>

<div class="language-cpp highlighter-rouge"><div class="code-header">
        <span data-label-text="Cpp"><i class="fas fa-code fa-fw small"></i></span>
      <button aria-label="copy" data-title-succeed="Copied!"><i class="far fa-clipboard"></i></button></div><div class="highlight"><code><table class="rouge-table"><tbody><tr><td class="rouge-gutter gl"><pre class="lineno">1
2
3
4
</pre></td><td class="rouge-code"><pre><span class="kt">void</span> <span class="nf">RecurMoveDisks</span><span class="p">(</span><span class="kt">int</span> <span class="n">n</span><span class="p">,</span> <span class="kt">int</span> <span class="n">from</span><span class="p">,</span> <span class="kt">int</span> <span class="n">temp</span><span class="p">,</span> <span class="kt">int</span> <span class="n">to</span><span class="p">)</span>
<span class="p">{</span>
  <span class="c1">//TODO:</span>
<span class="p">}</span>
</pre></td></tr></tbody></table></code></div></div>

<p>ㅤ</p>

<h2 id="규칙-찾기"><span class="me-2">규칙 찾기</span><a href="#규칙-찾기" class="anchor text-muted"><i class="fas fa-hashtag"></i></a></h2>

<h3 id="직접-규칙찾아보기"><span class="me-2">직접 규칙찾아보기</span><a href="#직접-규칙찾아보기" class="anchor text-muted"><i class="fas fa-hashtag"></i></a></h3>

<p>먼저 <a href="https://www.mathsisfun.com/games/towerofhanoi.html" target="_blank">플레이하러 가기</a> 에서 간단하게나마 3개부터 시작하여 차차 디스크 개수를 늘려가며 하노이의 탑을 플레이해보자.</p>

<p>하노이의 탑을 처음 접해본 입장에서는 3정도는 쉽게 풀다가 4, 5개 부터는 확실히, 매우 난해했다. 수십번을 플레이해봐도 도저히 어떻게 스탭들을 규칙적으로 밟아야 하는지 감도 못 잡은 채 문제 보기만 2시간을 하며 헛발질을 했다.</p>

<p>직감으로는 전혀 못풀것같은 느낌.. 규칙을 정리해야겠다는 생각을 했다.</p>

<div class="notice">
그래도 찾은 하나의 작은 가이드라인은,<br />
타겟 기둥으로 갈 때 가장 아래 디스크 제외, 나머지 상체부분을 일단 임시 기둥에 전부 옮겨놓고<br />
가장 아래 디스크를 제일 먼저 타겟 기둥으로 옮겨야 한다는 것<br />
<span class="faded_italic-text dimmed-text">(가장 바닥부터 차차 메꾼다는 느낌)</span>
</div>

<p>나는 이 추정으로 아래처럼 논리를 확장해갔다.</p>

<h3 id="논리-점화"><span class="me-2">논리 점화</span><a href="#논리-점화" class="anchor text-muted"><i class="fas fa-hashtag"></i></a></h3>

<p><code class="language-plaintext highlighter-rouge">디스크</code>가 4개 있다고 해보자. 전부<code class="language-plaintext highlighter-rouge">from</code>에서  <code class="language-plaintext highlighter-rouge">to</code>로 최소한의 횟수로 옮기는 것이 목표이다.</p>

<blockquote>
  <p>편의상 n-3, n-2, n-1, n 와 같이 쌓이는 것으로 표현</p>
</blockquote>

<p><a href="/../../images/2024-09-26-TowerOfHanoi/image-20240927003230815.png" class="popup img-link  shimmer"><img src="/../../images/2024-09-26-TowerOfHanoi/image-20240927003230815.png" alt="image-20240927003230815" style="zoom:33%;" loading="lazy"></a></p>

<p>가장 아래 디스크 <code class="language-plaintext highlighter-rouge">n-3</code>을 옮기려고 봤지만, 위 디스크  <code class="language-plaintext highlighter-rouge">n-2</code>, <code class="language-plaintext highlighter-rouge">n-1</code>, <code class="language-plaintext highlighter-rouge">n</code> 때문에 옮기지 못한다.<br />
위에 쌓여있는 디스크들을 <code class="language-plaintext highlighter-rouge">먼저</code>해결해보자.</p>

<blockquote>
  <p>편의상 잠시 n-3을 잠시 가려보자</p>
</blockquote>

<p><br /></p>

<p><a href="/../../images/2024-09-26-TowerOfHanoi/image-20240927003332669.png" class="popup img-link  shimmer"><img src="/../../images/2024-09-26-TowerOfHanoi/image-20240927003332669.png" alt="image-20240927003332669" style="zoom:33%;" loading="lazy"></a></p>

<p>가장 아래 디스크 <code class="language-plaintext highlighter-rouge">n-2</code>를 옮기려고 봤지만, 위 디스크  <code class="language-plaintext highlighter-rouge">n-1</code>, <code class="language-plaintext highlighter-rouge">n</code> 때문에 옮기지 못한다.<br />
위에 쌓여있는 디스크들을 먼저 해결해보자.</p>

<blockquote>
  <p>문제가 잘게 쪼개짐이 보인다.<br />
편의상 n-2를 잠시 가려보자</p>
</blockquote>

<p><br /></p>

<p><a href="/../../images/2024-09-26-TowerOfHanoi/image-20240927003423933.png" class="popup img-link  shimmer"><img src="/../../images/2024-09-26-TowerOfHanoi/image-20240927003423933.png" alt="image-20240927003423933" style="zoom:33%;" loading="lazy"></a></p>

<p>여기서부터는 마지막 두 디스크 <code class="language-plaintext highlighter-rouge">n-1</code>, <code class="language-plaintext highlighter-rouge">n</code>이남는다.<br />
<span class="faded_italic-text dimmed-text">또 <code class="language-plaintext highlighter-rouge">n-1</code>를 가려 마지막 하나 <code class="language-plaintext highlighter-rouge">n</code>을 남겨 따져보는 것은 아무런 의미가 없다.</span></p>

<p>결국 마지막 2개 디스크 간의 이동 논리가 이 문제 해결 과정의 <code class="language-plaintext highlighter-rouge">최소 단위 논리</code>가 된다.</p>

<p>결국 여기서 <code class="language-plaintext highlighter-rouge">from</code>부터 <code class="language-plaintext highlighter-rouge">to</code>까지의 이동과정을 보여라인데, 너무 쉽다.<br />
<code class="language-plaintext highlighter-rouge">n</code>을 잠시 <code class="language-plaintext highlighter-rouge">temp</code>에 두고 바닥 <code class="language-plaintext highlighter-rouge">n-1</code>을 <code class="language-plaintext highlighter-rouge">to</code>로 옮기고 <code class="language-plaintext highlighter-rouge">n</code>을 <code class="language-plaintext highlighter-rouge">to</code>에 있는 <code class="language-plaintext highlighter-rouge">n-1</code>에 쌓으면 된다.</p>

<p><a href="/../../images/2024-09-26-TowerOfHanoi/image-20240927004414573.png" class="popup img-link  shimmer"><img src="/../../images/2024-09-26-TowerOfHanoi/image-20240927004414573.png" alt="image-20240927004414573" loading="lazy"></a></p>

<hr />

<h3 id="관계식-세우기"><span class="me-2">관계식 세우기</span><a href="#관계식-세우기" class="anchor text-muted"><i class="fas fa-hashtag"></i></a></h3>

<p>위의 이동 논리는 디스크 이동의 <code class="language-plaintext highlighter-rouge">최소 논리 단위</code>이니 이동 양상을 관계식으로 세워보고<br />
다른 디스크의 개수 문제에서도 적용 되는지 따져보아 가설을 검증해나가는 귀납적 추론을 해봤다.</p>

<p>전제조건은 디스크의 개수는 2개일 때.<br />
아래 디스크를 <code class="language-plaintext highlighter-rouge">me</code>, 나머지 위의 디스크를 <code class="language-plaintext highlighter-rouge">head</code>, 시작점을 <code class="language-plaintext highlighter-rouge">f</code>, 임시 공간을  <code class="language-plaintext highlighter-rouge">tm</code>, 목적지를 <code class="language-plaintext highlighter-rouge">to</code>라고 하자.</p>

<p><a href="/../../images/2024-09-26-TowerOfHanoi/image-20240927012706352.png" class="popup img-link  shimmer"><img src="/../../images/2024-09-26-TowerOfHanoi/image-20240927012706352.png" alt="image-20240927012706352" loading="lazy"></a></p>

<p>이때 각 3단계 마다 이루어지는 이동을 다음과 같이 정리해볼 수 있다.</p>

<blockquote>
  <p>1st : <code class="language-plaintext highlighter-rouge">head</code> → <code class="language-plaintext highlighter-rouge">tm</code><br />
2nd: <code class="language-plaintext highlighter-rouge">me</code> → <code class="language-plaintext highlighter-rouge">to</code><br />
3rd: <code class="language-plaintext highlighter-rouge">tm</code>에 있던 me → <code class="language-plaintext highlighter-rouge">to</code></p>
</blockquote>

<ul>
  <li>그리고 다음을 관계식으로 세워봤다
    <ul>
      <li>1st : <code class="language-plaintext highlighter-rouge">head</code> → <code class="language-plaintext highlighter-rouge">tm</code></li>
      <li>2nd : <code class="language-plaintext highlighter-rouge">me</code> → <code class="language-plaintext highlighter-rouge">to</code></li>
      <li>3rd  : <code class="language-plaintext highlighter-rouge">tm</code> → <code class="language-plaintext highlighter-rouge">to</code> <span class="faded_italic-text dimmed-text">tm에 있던 것이 to로 감</span></li>
      <li>+ 기호는 단계 구분이다. 1st, 2nd, 3rd…</li>
    </ul>
  </li>
</ul>

<p><a href="/../../images/2024-09-26-TowerOfHanoi/image-20240927021418537.png" class="popup img-link  shimmer"><img src="/../../images/2024-09-26-TowerOfHanoi/image-20240927021418537.png" alt="IMG_1114" loading="lazy"></a></p>

<hr />

<h3 id="disk가-3개일-때-귀납추론-검증"><span class="me-2">disk가 3개일 때 귀납추론 검증</span><a href="#disk가-3개일-때-귀납추론-검증" class="anchor text-muted"><i class="fas fa-hashtag"></i></a></h3>

<div class="flex-container">
    <a href="/../../images/2024-09-26-TowerOfHanoi/image-20240927020456649.png" class="popup img-link flex-image shimmer"><img src="/../../images/2024-09-26-TowerOfHanoi/image-20240927020456649.png" alt="설명"  loading="lazy"></a>
    <div class="flex-text">
여기서 가장 아래 디스크 n-2가 me이고,<br />나머지 윗 부분 n-1, n은 head이다.
    </div>
</div>

<p><br /></p>

<p><a href="/../../images/2024-09-26-TowerOfHanoi/image-20240927055334434.png" class="popup img-link  shimmer"><img src="/../../images/2024-09-26-TowerOfHanoi/image-20240927055334434.png" alt="image-20240927055334434" loading="lazy"></a></p>

<blockquote>
  <p>첫 번째 단계 (i) 는 다음과 같이 전개된다. 첫째항에서 시작점에 디스크가 2개가 있으니 또 관계식을 적용해준다.<br />
여기선 첫째항, 셋째항에 또 관계식 적용이 가능하다.<span class="faded_italic-text dimmed-text">디스크 한 개의 이동들로 다 쪼갠다.</span></p>

  <p>항에서 또 관계식으로 분화하며 가지가 나듯이 이는 나중에 쓰게 될 재귀함수의 개념과 유사하다.</p>
</blockquote>

<p><br /></p>

<p><a href="/../../images/2024-09-26-TowerOfHanoi/image-20240927032323076.png" class="popup img-link  shimmer"><img src="/../../images/2024-09-26-TowerOfHanoi/image-20240927032323076.png" alt="image-20240927032323076" loading="lazy"></a></p>

<blockquote>
  <p>(i)의 첫번째 항에 관계식을 또 적용하면 (ii)가 생긴다. 이는 재귀함수가 실행될 때 마다 스택이 쌓이는 것과 같다.</p>

  <p>먼저 (i)의 첫째항인 (head<span class="faded_italic-text dimmed-text">from자리</span> → tm)이 (ii)에선 &lt;f → to&gt;로 대치된다.<span class="faded_italic-text dimmed-text">(i)의 (f→tm) ⇒ (ii)의 &lt;f→to&gt; = … </span> <br />
<span class="faded_italic-text dimmed-text">! head는 어차피 from에서 꺼내는 디스크이니 같다고 봐도 무방하다</span></p>

  <p>즉,</p>

  <ul>
    <li>(ii)의 f는 (i)의 f와 같다.</li>
    <li>(ii)의 to는 (i)의 tm와 같게 된다.<span class="faded_italic-text dimmed-text">(ii)에서 to로의 이동을 전역적으로 보면 원래의 tm에 이동한다는 말과 같다.</span></li>
    <li>(ii)의 tm은 남는 (i)의 to와 같게 된다. <span class="faded_italic-text dimmed-text">어차피 f, to가 아니면 남는게 tm이다. + 일대일대응</span></li>
  </ul>
</blockquote>

<p>결국 (ii)의 to는 원래 기둥 3개에서<span class="faded_italic-text dimmed-text">기둥[0], 기둥[1], 기둥[2]</span> tm자리 인덱스[1] 자리를 가리키게 된다.<br />
<strong>나의 f, tm, to와 윗쪽의 f, tm, to는 다른 자리, 다른 스택임을 주의하자.</strong></p>

<blockquote>
  <p>🌟왜 대응관계의 꼬임이 일어나는가??<br />
<strong>(i)의 첫째항인 (f<span class="faded_italic-text dimmed-text"> = head</span> →tm)에서 f, tm이 (ii)의 &lt;f → to&gt; = … 의 각각의 인자에 들어가기 때문이다.</strong> <br />
따라서 원래 (i)의 f는 (ii)에서 f로 다뤄지게되는 반면,<br />
원래 (i)의 tm은 (ii)에서는 to로 다뤄지게 된다.<br />
<span class="faded_italic-text dimmed-text"><em>f는 시작점, to는 목적지의 의미이다</em></span></p>
</blockquote>

<p>헷갈리면 하단의 오랜지색 인덱스 값을 참고하자. 글로벌 변수처럼 절대값이기 때문이다.</p>

<blockquote>
  <p>(i) 첫째항에서 가지처럼 뻗어난 (ii)의 첫째항은 비로소 디스크 하나 짜리가 보인다. 이는 우리가 이동시켜 줄 수 있는 단위이며 더 이상 쪼갤 수 없다.</p>

  <p>(ii)의 세 항별 디스크 이동을 살펴보면 다음과 같다.</p>
</blockquote>

<p><br /></p>

<p><a href="/../../images/2024-09-26-TowerOfHanoi/image-20240927061130729.png" class="popup img-link  shimmer"><img src="/../../images/2024-09-26-TowerOfHanoi/image-20240927061130729.png" alt="image-20240927061130729" loading="lazy"></a></p>

<blockquote>
  <p>(ii) 시행이 모두 끝나면 (i)의 첫째항으로 복귀한다. 재귀함수의 뻗었던 가지 거둬들이기랑 비슷해보인다.<span class="faded_italic-text dimmed-text">반환을 이용한 복귀</span></p>

  <p>(i)의 둘째항으로 넘어가 시행하면 다음과 같다.</p>
</blockquote>

<p><a href="/../../images/2024-09-26-TowerOfHanoi/image-20240927052142168.png" class="popup img-link  shimmer"><img src="/../../images/2024-09-26-TowerOfHanoi/image-20240927052142168.png" alt="image-20240927052142168" style="zoom:33%;" loading="lazy"></a></p>

<blockquote>
  <p>(i) 둘째항 시행이 끝나면 (i) 셋째항으로 넘어가고 (i)의 셋째항 역시 시작점에 디스크 두 개가 있으니 이동 관계식을 적용시키면<br />
(iii)로 전개된다.</p>
</blockquote>

<blockquote>
  <p>(i)의 셋째항인 (tm → to)가 (iii)의 &lt;f → to&gt;로 대치된다. <span class="faded_italic-text dimmed-text">(i)의 (tm→to) ⇒ (iii)의 &lt;f→to&gt; = …</span></p>

  <p>즉,</p>

  <ul>
    <li>
      <p>(iii)의 f자리에 (i)의 tm이 들어오니, (iii)의 f = (i)의 tm</p>

      <p>(iii)의 to자리에 (i)의 to가 들어오니, (iii)의 to = (i)의 to</p>
    </li>
    <li>
      <p>(iii)의 tm = (i)의 f</p>
    </li>
  </ul>

  <p>(iii)의 세 항별 이동을 살펴보면 다음과 같다.</p>
</blockquote>

<p><a href="/../../images/2024-09-26-TowerOfHanoi/image-20240927061852108.png" class="popup img-link  shimmer"><img src="/../../images/2024-09-26-TowerOfHanoi/image-20240927061852108.png" alt="image-20240927061852108" loading="lazy"></a></p>

<p>(iii)이 끝나면 (i)의 셋째항으로 돌아가고 결국 전부 끝나게 되어 가정한대로 살펴본 예시 케이스에서도 이동 관계식은 적용됐다.</p>

<hr />

<h3 id="disk의-개수가-4-개일-때"><span class="me-2">disk의 개수가 4+… 개일 때</span><a href="#disk의-개수가-4-개일-때" class="anchor text-muted"><i class="fas fa-hashtag"></i></a></h3>

<p>4개일 때도 역시 같은 방법으로 하면 최소횟수로 완벽하게 이동된다. 다만 4개이므로 도식을 그리는 데는 좀 걸린다.<br />
5, 6개도 역시 된다. <br />
물론 위에서 소개한 꼬리에 꼬리를 무는 <code class="language-plaintext highlighter-rouge">나머지 위 디스크들을 잠시 다른 데로 옮기기</code> 가 이 식과 이미 같다.</p>

<p><a href="/../../images/2024-09-26-TowerOfHanoi/image-20240927062004856.png" class="popup img-link  shimmer"><img src="/../../images/2024-09-26-TowerOfHanoi/image-20240927062004856.png" alt="image-20240927062004856" style="zoom:33%;" loading="lazy"></a></p>

<hr />

<h2 id="코드-구현"><span class="me-2">코드 구현</span><a href="#코드-구현" class="anchor text-muted"><i class="fas fa-hashtag"></i></a></h2>

<h3 id="재귀-부분"><span class="me-2">재귀 부분</span><a href="#재귀-부분" class="anchor text-muted"><i class="fas fa-hashtag"></i></a></h3>

<div class="language-c++ highlighter-rouge"><div class="code-header">
        <span data-label-text="C++"><i class="fas fa-code fa-fw small"></i></span>
      <button aria-label="copy" data-title-succeed="Copied!"><i class="far fa-clipboard"></i></button></div><div class="highlight"><code><table class="rouge-table"><tbody><tr><td class="rouge-gutter gl"><pre class="lineno">1
2
</pre></td><td class="rouge-code"><pre><span class="cp">#int main()
</span><span class="n">RecurMoveDisks</span><span class="p">(</span><span class="n">num_disks</span><span class="p">,</span> <span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">2</span><span class="p">);</span>
</pre></td></tr></tbody></table></code></div></div>

<p>인자로 <code class="language-plaintext highlighter-rouge">num_disks</code>는 3, <code class="language-plaintext highlighter-rouge">from</code>은 인덱스 0, <code class="language-plaintext highlighter-rouge">tm</code>, <code class="language-plaintext highlighter-rouge">to</code>는 1, 2를 넣는다.<br />
구현에는 재귀함수를 쓰지만 무한재귀를 하지는 않을 것이기 때문에 시작과 종료 condition역시 잘 정해야 한다.</p>

<p>일단 내가 구현하려 했을 때 딱 처음에 했던 생각은 아래 사진의 노란색 하이라이트 된 화살표처럼<br />
한 항에서 또 재귀적으로 새로운 식이 열리는 코드를 어떻게 구현할 것인지였다.</p>

<p><a href="/../../images/2024-09-26-TowerOfHanoi/image-20240927064157833.png" class="popup img-link  shimmer"><img src="/../../images/2024-09-26-TowerOfHanoi/image-20240927064157833.png" alt="image-20240927064157833" loading="lazy"></a></p>

<p>그래서 일단 <code class="language-plaintext highlighter-rouge">RecurMoveDisks()</code>를 3개를 나란히 두었다. 하지만 재귀가 필요한 항은 첫째항과 셋째항이다.<br />
둘째항은 디스크 1개를 옮기는 것이므로 디스크 하나를 옮기는 <code class="language-plaintext highlighter-rouge">MoveDisk()</code>를 사용한다.</p>

<div class="language-c++ highlighter-rouge"><div class="code-header">
        <span data-label-text="C++"><i class="fas fa-code fa-fw small"></i></span>
      <button aria-label="copy" data-title-succeed="Copied!"><i class="far fa-clipboard"></i></button></div><div class="highlight"><code><table class="rouge-table"><tbody><tr><td class="rouge-gutter gl"><pre class="lineno">1
2
3
</pre></td><td class="rouge-code"><pre>	<span class="n">RecurMoveDisks</span><span class="p">(</span> <span class="p">,</span> <span class="p">,</span> <span class="p">,</span> <span class="p">);</span> <span class="c1">// 1st term</span>
	<span class="n">MoveDisk</span><span class="p">(</span> <span class="p">,);</span> <span class="c1">// 2nd term</span>
	<span class="n">RecurMoveDisks</span><span class="p">(</span> <span class="p">,</span> <span class="p">,</span> <span class="p">,</span> <span class="p">);</span> <span class="c1">// 3rd term</span>
</pre></td></tr></tbody></table></code></div></div>

<p><br /></p>

<p>이제는 인수들을 채워야한다. 첫째항인 1st term을 다시 위의 사진에서 한 번 보자.<br />
새로 재귀식을 열 때 &lt;f→to&gt;에서 <code class="language-plaintext highlighter-rouge">f</code>인자에 자신의 <code class="language-plaintext highlighter-rouge">f</code>를 넣고, <code class="language-plaintext highlighter-rouge">to</code>인자에 자신의 <code class="language-plaintext highlighter-rouge">tm</code>를 넣는다.</p>

<p><code class="language-plaintext highlighter-rouge">→ RecurMoveDisks(..., from, to, temp);</code><span class="faded_italic-text dimmed-text">남는 인수자리는 to거 ㅎㅎ..</span></p>

<p><br /></p>

<p>둘째항인 2nd term은 <code class="language-plaintext highlighter-rouge">me</code> → <code class="language-plaintext highlighter-rouge">to</code>를 수행하는데, <code class="language-plaintext highlighter-rouge">me</code>는 <code class="language-plaintext highlighter-rouge">from</code>자리에 있기 때문에 <br />
<code class="language-plaintext highlighter-rouge">from</code>인수에 자신의 <code class="language-plaintext highlighter-rouge">from</code>을, <code class="language-plaintext highlighter-rouge">to</code>인수에 자신의 <code class="language-plaintext highlighter-rouge">to</code>를 넣는다.<br />
<code class="language-plaintext highlighter-rouge">→ MoveDisk(from, to);</code></p>

<p><br /></p>

<p>셋째항인 3nd term은 <code class="language-plaintext highlighter-rouge">tm</code> → <code class="language-plaintext highlighter-rouge">to</code>를 수행한다.<br />
새로 재귀식을 열 때 &lt;f→to&gt;에서 <code class="language-plaintext highlighter-rouge">f</code>인자에 자신의 <code class="language-plaintext highlighter-rouge">tm</code>를, <code class="language-plaintext highlighter-rouge">to</code>인자에 자신의 <code class="language-plaintext highlighter-rouge">to</code>를 넣는다.</p>

<p><code class="language-plaintext highlighter-rouge">→ RecurMoveDisks(..., temp, from, to);</code></p>

<hr />

<h3 id="재귀-종료-조건-부분"><span class="me-2">재귀 종료 조건 부분</span><a href="#재귀-종료-조건-부분" class="anchor text-muted"><i class="fas fa-hashtag"></i></a></h3>

<p>도식을 보면 알 수 있듯이 무한히 재귀하지 않듯이 좀 자세히 보면 언제 재귀를 멈추는지 알 수 있다.</p>

<p>일단 가장 처음 함수가 실행될 때 n에는 3이 들어온다. <br />
초기에는 (i)로 전개가 되는데 3가 매칭되는 요소는 &lt;f→to&gt;에서 <code class="language-plaintext highlighter-rouge">f</code>의 디스크 개수이다. <br />
따라서 이와 관련하여 종료 조건을 짜면 될 것 같았다.</p>

<p>그렇다면 재귀함수를 실행하는 부분 위에 조건 검사식을 둬야함을 알 수 있다. 아래는 최종코드이다.</p>

<div class="language-c++ highlighter-rouge"><div class="code-header">
        <span data-label-text="C++"><i class="fas fa-code fa-fw small"></i></span>
      <button aria-label="copy" data-title-succeed="Copied!"><i class="far fa-clipboard"></i></button></div><div class="highlight"><code><table class="rouge-table"><tbody><tr><td class="rouge-gutter gl"><pre class="lineno">1
2
3
4
5
6
7
8
9
10
11
12
13
</pre></td><td class="rouge-code"><pre><span class="kt">void</span> <span class="nf">RecurMoveDisks</span><span class="p">(</span><span class="kt">int</span> <span class="n">n</span><span class="p">,</span> <span class="kt">int</span> <span class="n">from</span><span class="p">,</span> <span class="kt">int</span> <span class="n">temp</span><span class="p">,</span> <span class="kt">int</span> <span class="n">to</span><span class="p">)</span>
<span class="p">{</span>
	<span class="k">if</span> <span class="p">(</span><span class="n">n</span> <span class="o">==</span> <span class="mi">1</span><span class="p">)</span>
	<span class="p">{</span>
		<span class="n">MoveDisk</span><span class="p">(</span><span class="n">from</span><span class="p">,</span> <span class="n">to</span><span class="p">);</span>
		<span class="k">return</span><span class="p">;</span>
	<span class="p">}</span>

	<span class="n">RecurMoveDisks</span><span class="p">(</span><span class="n">n</span> <span class="o">-</span> <span class="mi">1</span><span class="p">,</span> <span class="n">from</span><span class="p">,</span> <span class="n">to</span><span class="p">,</span><span class="n">temp</span><span class="p">);</span> <span class="c1">// 1st term</span>
	<span class="n">MoveDisk</span><span class="p">(</span><span class="n">from</span><span class="p">,</span> <span class="n">to</span><span class="p">);</span> <span class="c1">// 2nd term</span>
	<span class="n">RecurMoveDisks</span><span class="p">(</span><span class="n">n</span> <span class="o">-</span> <span class="mi">1</span><span class="p">,</span> <span class="n">temp</span><span class="p">,</span> <span class="n">from</span><span class="p">,</span> <span class="n">to</span><span class="p">);</span> <span class="c1">// 3rd term</span>

<span class="p">}</span>
</pre></td></tr></tbody></table></code></div></div>

<ul>
  <li>
    <p>재귀에서 재귀로 또 들어갈 때 무슨 인자를 넣어주는지는 어려울 것 없다. 내 <code class="language-plaintext highlighter-rouge">from</code>, <code class="language-plaintext highlighter-rouge">temp</code>, <code class="language-plaintext highlighter-rouge">to</code>등이 새로운 재귀식에서 어떤 자리를 맡게되는지를 따지는 것이다.</p>
  </li>
  <li>
    <p>(i)는 &lt;f→to&gt;에서 <code class="language-plaintext highlighter-rouge">f</code>가 디스크를 3개 가지고 있다. 다음 재귀 때 <code class="language-plaintext highlighter-rouge">n-1</code>을 해주자.</p>
  </li>
  <li>
    <p>(ii)는 &lt;f→to&gt;부분에서 <code class="language-plaintext highlighter-rouge">f</code>가 디스크 2개를 가지고 있다. 재귀는 여기까지만 되어야 한다. 일단 다음 재귀 때 역시 <code class="language-plaintext highlighter-rouge">n-1</code>을 해주자.</p>

    <ul>
      <li>
        <p>그렇다면 예컨대 (ii)의 첫째항에서는 다시 <code class="language-plaintext highlighter-rouge">n</code>을 <code class="language-plaintext highlighter-rouge">-1</code>을 한 재귀식이 실행된다.</p>
      </li>
      <li>
        <p>일단 새로 열린 재귀함수에서 <code class="language-plaintext highlighter-rouge">n</code>은 1이 된다. 여기서는 재귀를 멈춰야 하므로 <code class="language-plaintext highlighter-rouge">if (n==1)</code>을 걸고 맞다면<br />
<code class="language-plaintext highlighter-rouge">MoveDisk(from, to);</code>을 하고 <code class="language-plaintext highlighter-rouge">return</code>을 하여 종료한다.<br />
<a href="/../../images/2024-09-26-TowerOfHanoi/image-20240927072327484.png" class="popup img-link  shimmer"><img src="/../../images/2024-09-26-TowerOfHanoi/image-20240927072327484.png" alt="image-20240927072327484" loading="lazy"></a><br />
빨간색 하이라이트 부분을 보면 시작점은 <code class="language-plaintext highlighter-rouge">f</code>, 도착지는 <code class="language-plaintext highlighter-rouge">tm</code>이다.<br />
일단 여기서 새로 열리는 재귀식에 <code class="language-plaintext highlighter-rouge">f</code>부분에 자기 <code class="language-plaintext highlighter-rouge">f</code>를, <code class="language-plaintext highlighter-rouge">to</code>부분에 자기 <code class="language-plaintext highlighter-rouge">tm</code>을 넣어주면 된다. = 이동을 f에서 tm로 하겠단 의미.  <br />
결국엔 새로 열린 재귀식에서 시작점 부분은 자기 <code class="language-plaintext highlighter-rouge">f</code>가, 도착지 부분에 자기 <code class="language-plaintext highlighter-rouge">tm</code>을 잘 넣었다. 이후부터는 각각 <code class="language-plaintext highlighter-rouge">f</code>, <code class="language-plaintext highlighter-rouge">to</code>로 대치된다.</p>

        <p>하지만 재귀 종료조건 <code class="language-plaintext highlighter-rouge">if (n==1)</code>에 걸린다. 그 후 <code class="language-plaintext highlighter-rouge">MoveDisk()</code>에는 원했던 시작점, 도착지인 <code class="language-plaintext highlighter-rouge">f</code>, <code class="language-plaintext highlighter-rouge">to</code>를 넣어주면 된다.<br />
<code class="language-plaintext highlighter-rouge">→ MoveDisk(from , to);</code> <br />
이후 <code class="language-plaintext highlighter-rouge">return</code>을 만나며 되돌아간다.</p>
      </li>
    </ul>
  </li>
</ul>

<p>이렇게 첫째항, 셋째항도 해결했다.</p>

<p>첫, 둘, 셋째 항들을 직렬로 배열했으니 첫째항의 가지들이 쭉 나갔다가 다시 거둬들여지고, 차례대로 둘.셋째도 마찬가지고 결국 함수가 끝난다.</p>

<hr />

<p>주저리 주저리… 오지랖이 넓은 긴 설명이지만 하루동안 심히 시름했던 문제이었기에 내가 어떻게 풀었는지 자세히 기록하고싶었다.</p>

  </div>

  <div class="post-tail-wrapper text-muted">
    <!-- categories -->
    
      <div class="post-meta mb-3">
        <i class="far fa-folder-open fa-fw me-1"></i>
        
          <a href="/categories/data-structures/">Data Structures</a>,
          <a href="/categories/hjm-data-structures/">HJM Data Structures</a>,
          <a href="/categories/cpp/">Cpp</a>
      </div>
    

    <!-- tags -->
    
      <div class="post-tags">
        <i class="fa fa-tags fa-fw me-1"></i>
        
          <a
            href="/tags/data-structures/"
            class="post-tag no-text-decoration"
          >Data Structures</a>
        
          <a
            href="/tags/cpp/"
            class="post-tag no-text-decoration"
          >Cpp</a>
        
      </div>
    

    <div
      class="
        post-tail-bottom
        d-flex justify-content-between align-items-center mt-5 pb-2
      "
    >
      <div class="license-wrapper">
        
          

          This post is licensed under 
        <a href="https://creativecommons.org/licenses/by/4.0/">
          CC BY 4.0
        </a>
         by the author.
        
      </div>

      <!-- Post sharing snippet -->

<div class="share-wrapper d-flex align-items-center">
  <span class="share-label text-muted">Share</span>
  <span class="share-icons">
    
    
    

    

      

      <a href="https://twitter.com/intent/tweet?text=Tower%20of%20Hanoi,%20HJM%20Data%20Stuctures%204-3%20-%20%D0%BA%D1%83%D1%81%D0%BE%D0%BA%20%D1%81%D0%B5%D0%BB%D1%91%D0%B4%D0%BA%D0%B0&url=http%3A%2F%2Flocalhost%3A4000%2Fposts%2FTowerOfHanoi%2F" target="_blank" rel="noopener" data-bs-toggle="tooltip" data-bs-placement="top" title="Twitter" aria-label="Twitter">
        <i class="fa-fw fa-brands fa-square-x-twitter"></i>
      </a>
    

      

      <a href="https://www.facebook.com/sharer/sharer.php?title=Tower%20of%20Hanoi,%20HJM%20Data%20Stuctures%204-3%20-%20%D0%BA%D1%83%D1%81%D0%BE%D0%BA%20%D1%81%D0%B5%D0%BB%D1%91%D0%B4%D0%BA%D0%B0&u=http%3A%2F%2Flocalhost%3A4000%2Fposts%2FTowerOfHanoi%2F" target="_blank" rel="noopener" data-bs-toggle="tooltip" data-bs-placement="top" title="Facebook" aria-label="Facebook">
        <i class="fa-fw fab fa-facebook-square"></i>
      </a>
    

      

      <a href="https://t.me/share/url?url=http%3A%2F%2Flocalhost%3A4000%2Fposts%2FTowerOfHanoi%2F&text=Tower%20of%20Hanoi,%20HJM%20Data%20Stuctures%204-3%20-%20%D0%BA%D1%83%D1%81%D0%BE%D0%BA%20%D1%81%D0%B5%D0%BB%D1%91%D0%B4%D0%BA%D0%B0" target="_blank" rel="noopener" data-bs-toggle="tooltip" data-bs-placement="top" title="Telegram" aria-label="Telegram">
        <i class="fa-fw fab fa-telegram"></i>
      </a>
    

    <button
      id="copy-link"
      aria-label="Copy link"
      class="btn small"
      data-bs-toggle="tooltip"
      data-bs-placement="top"
      title="Copy link"
      data-title-succeed="Link copied successfully!"
    >
      <i class="fa-fw fas fa-link pe-none fs-6"></i>
    </button>
  </span>
</div>

    </div>
    <!-- .post-tail-bottom -->
  </div>
  <!-- div.post-tail-wrapper -->
</article>