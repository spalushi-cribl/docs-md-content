# Hellish CSS test page, now with graphics too


Everything that could go wrong, should go wrong here

---

[Heading weights are currently all 700, per our Docusaurus default theme – can restore variations later to match the original readme.io weights listed here:]

# Heading 1 = PageTitle = 2 rem, was weight 500 default => 32px
## Heading 2 = 1.75 rem, was weight 500 default => 28px
### Heading 3 = 1.4 rem, was weight 400 default => was 16px, now ~24px
#### Heading 4 = 1.3 rem, was weight 400 default => ~20px
##### Heading 5 = 1.15 rem, was weight 600 default  => ~18px
###### Heading 6 = 1.05 rem, was weight 600 default => ~17px
Body text = 1.0 rem, now weight 400 default => 16px

## Heading 1

There are at least **two** key factors that will determine the type of Cribl Stream deployment in your environment: Whether you want a [Single Instance Deployment](/stream/deploy-single-instance) or a [Distributed Deployment](/stream/deploy-distributed).

> Type: **Push**  |  TLS Support: **YES**
>
{.box .info}

### Heading 2

### API Header [block]: Functions as H2, but Looks Prettier in Editor

* Bullet A
* Bullet B

<img style={{marginLeft: "2em", marginBottom: "1em"}} alt="Monitoring screen" src={require("./_img/Monitoring-screen.png").defeault}></img>
<span style={{fontSize: ".93em", fontStyle: "italic", textAlign: "center"}}>
<p>Indented graphic, hacked in HTML</p>
</span>

* Bullet C

## Heading 1

### Heading 2

^ Stacked heads!

#### Heading 3

Unicorn typewriter air plant kinfolk. Asymmetrical master cleanse PBR&B glossier green juice magna cold-brew chia butcher keffiyeh artisan. Irure pitchfork wolf air plant farm-to-table listicle.

##### Heading 4

Flexitarian paleo deserunt. Etsy whatever labore biodiesel pour-over, keto est bushwick normcore occaecat YOLO DIY leggings,la croix occupy gluten-free incididunt lo-fi slow-carb chillwave jianbing sartorial man bun.

<blockquote className="callout callout_info" theme="ℹ">  
	<h5 className="callout-heading false">Fake callout, tagged h5<span className="callout-icon">ℹ</span>
	</h5>
	<p>The <strong>Disable Field Metrics</strong> setting (in <strong>Settings &gt; System &gt; General Settings &gt; Limits</strong>) applies only to metrics sent to the Master Node. When the <strong>Cribl Internal</strong> Source is enabled, Cribl Stream ignores this <strong>Disable Field Metrics</strong> setting, and full-fidelity data will flow down the Routes.
  	</p>
</blockquote>

###### Heading 5

You can use these things:

![Time Cube, at 70% magnification, no border](time-cube.ad6eebb989.png)
{scale="70%"}



##### Heading 4

This, my friends, is a *real* graphic:

![Ilsa the puppy was sad until she discovered Cribl Stream!](puppy.2b46392a04.jpg)

Here'a another *real* graphic, without a caption:

![](log-click-insert-field.a5ffc1468c.png)

Blah blah blah body text after graphic; Williamsburg tumblr microdosing dreamcatcher trust fundsnackwave semiotics. 

Here's a "real" graphic *with* a caption:

![I gotta name, I gotta name.](log-expression-autocomplete.b64138c5af.png)

###### Heading 5

1. Something
2. And another thing

<div style={{marginLeft: "2em"}}>
<blockquote className="callout callout_info" theme="ℹ">
  
	<h5 className="callout-heading false">Fake callout, tagged h5<span className="callout-icon">ℹ</span>
	</h5>
  
<p>This Splunk HEC implementation is an <strong>event</strong> (i.e., not <strong>raw</strong>) endpoint. For details, see <a href="https://docs.splunk.com/Documentation/Splunk/latest/Data/UsetheHTTPEventCollector">Splunk's documentation</a>. To send data to it from a HEC client, use either <code>/services/collector</code> or <code>/services/collector/event</code>. (See the <a href="#examples">examples</a> below.)
  </p>
  <p>This implementation is <strong>superseded</strong> by the Cribl <a href="/stream/2.4/sources-splunk-hec">Splunk HEC</a> Source.
	</p>
  
  </blockquote>
</div>

#### Callouts with Headings

These, my friends, are *real* callouts: 

> ##### info: Whatev's
>
> Throughout these guidelines, we assume that 1 physical core is equivalent to 2 `__raw` virtual/hyperthreaded CPUs (vCPUs).
>
{.box .info}

> ##### warning: Cribl App for Splunk Deprecation Notice!
>
> Click [here](/stream/deploy-splunkapp). This app is `kapu`!
>
{.box .warning}

> ##### success: Mālie
>
> E komo mai, bruddah! The future is `truthy`.
>
{.box .success}

> ##### danger: Avertissement
>
> En délit, à cause d'aucune utilisation de la belle langue française. Returns `noop`.
>
{.box .danger}

> ##### cloud: Get Offa My Cloud
>
> No you can use this feature on a Cribl-managed Worker.
>
{.box .cloud}

> ##### windows: Where Do You Want to Go Today?
>
> Start me up.
>
{.box .windows}

#### Callouts Without Headings

These are "real" callouts *without* headings. Check that icons vertically align with first line of body text:

> Farm-to-table kale chips cloud bread, woke duis microdosing whatever edison bulb.
>
{.box .info}

> Kombucha 8-bit tumeric quinoa cronut heirloom viral mumblecore letterpress kale chips.
>
{.box .warning}

> Gluten-free copper mug lo-fi vinyl chartreuse chillwave chicharrones actually snackwave.
>
{.box .success}

> Echo park everyday carry succulents pabst street art ethical etsy. Quinoa mixtape tilde bicycle rights locavore craft beer tattooed food truck.
>
{.box .danger}

> No you can use this feature on a Cribl-managed Worker.
>
{.box .cloud}

> Start me up.
>
{.box .windows}

#### Fake HTML Callouts {#fake-callouts}

These callouts are rendered in HTML so that we can indent them 2em, 4 em, etc., to left-align them with list items.

<h4> Mit Headings</h4>

<div style={{marginLeft: "2em"}}>
	<blockquote className="callout callout_info" theme="ℹ">
<h3 className="callout-heading false">Gort<span className="callout-icon">ℹ</span></h3>
		<p>Farm-to-table kale chips cloud bread, woke duis microdosing whatever edison bulb.</p>
	</blockquote>
</div>

<div style={{marginLeft: "2em"}}>
  <blockquote className="callout callout_warn" theme="🚧">  
	<h3 className="callout-heading false">Klaatu<span className="callout-icon">⚠️</span>
	</h3>
  	<p>Kombucha 8-bit tumeric quinoa cronut heirloom viral mumblecore letterpress kale chips.</p>
  </blockquote>
</div>

<div style={{marginLeft: "2em"}}>
  <blockquote className="callout callout_okay" theme="👍">  
	<h3 className="callout-heading false">Barata<span className="callout-icon">✅</span>
	</h3>
  	<p>Gluten-free copper mug lo-fi vinyl chartreuse chillwave chicharrones actually snackwave.</p>
  </blockquote>
</div>

<div style={{marginLeft: "2em"}}>
  <blockquote className="callout callout_error" theme="❗️">  
	<h3 className="callout-heading false">Nikto<span className="callout-icon">❗️</span>
	</h3>
  	<p>Echo park everyday carry succulents pabst street art ethical etsy. Quinoa mixtape tilde bicycle rights locavore craft beer tattooed food truck.</p>
  </blockquote>
</div>

<h4> Mit Out Headings</h4>

<div style={{marginLeft: "2em"}}>
	<blockquote className="callout callout_info" theme="ℹ">
		<h3 className="callout-heading empty html-callout" style={{marginTop: "-0.5em"}}><span className="callout-icon">ℹ</span></h3>
		<p>Farm-to-table kale chips cloud bread, woke duis microdosing whatever edison bulb.</p>
	</blockquote>
</div>

<div style={{marginLeft: "2em"}}>
  <blockquote className="callout callout_warn" theme="🚧">  
	<h3 className="callout-heading empty html-callout" style={{marginTop: "0.4em"}}><span className="callout-icon">⚠️</span>
	</h3>
  	<p>Kombucha 8-bit tumeric quinoa cronut heirloom viral mumblecore letterpress kale chips.</p>
  </blockquote>
</div>

<div style={{marginLeft: "2em"}}>
  <blockquote className="callout callout_okay" theme="👍">  
	<h3 className="callout-heading empty html-callout" style={{marginTop: "0.4em"}}><span className="callout-icon">✅</span>
	</h3>
  	<p>Gluten-free copper mug lo-fi vinyl chartreuse chillwave chicharrones actually snackwave.</p>
  </blockquote>
</div>

<div style={{marginLeft: "2em"}}>
  <blockquote className="callout callout_error" theme="❗️">  
	<h3 className="callout-heading empty html-callout" style={{marginTop: "0.4em"}}><span className="callout-icon">❗️</span>
	</h3>
  	<p>Echo park everyday carry succulents pabst street art ethical etsy. Quinoa mixtape tilde bicycle rights locavore craft beer tattooed food truck.</p>
  </blockquote>
</div>

#### Callouts with Lists

Bullets and numbers should line up with the freaking body text: 

> ##### Keytar salvia biodiesel banh mi, hashtag tilde thundercats chillwave 90's
>
> Small batch vaporware seitan selfies vaporware cronut cardigan.
> 
> 1. Man bun franzen VHS poke YOLO.
> 
> 2. Post-ironic lo-fi hashtag la croix vaporware letterpress organic chambray.
> 
> * Raw denim bicycle rights trust fund fingerstache vice.
> 
> * Heirloom thundercats keytar. 
> 
> * Lumbersexual cold-pressed woke coloring book truffaut occupy mumblecore.
>
{.box .info}

![This is an .SVG!](load-balancing.cc767fe034.svg)

![This is a .PNG!](load-balancing.0a911c84e5.png)

![This is a bigger .PNG, the .PNG heard 'round the world!](load-balancing_lg.8f188ace83.png)

### Heading 2 Below Figure

``` {title="Recursive!"}
/* h1, h2; overline UNLESS h1 @ Logo, Title, or right below Title's horizontal rule: */
h1:nth-of-type(n+2), h2:nth-of-type(n+2), blockquote + h1, blockquote + h2, 
p + h1, p + h2/* , table + h1, table + h2, figure + h1, figure+h2 */ { 
  border-top: 1px solid #dfe2e5; 
}
```

## Heading 1 Below Code Block

``` {title="Recursive!"}
/* h1, h2; overline UNLESS h1 @ Logo, Title, or right below Title's horizontal rule: */
h1:nth-of-type(n+2), h2:nth-of-type(n+2), blockquote + h1, blockquote + h2, 
p + h1, p + h2/* , table + h1, table + h2, figure + h1, figure+h2 */ { 
  border-top: 1px solid #dfe2e5; 
}
```

## Heading 2 Below Code Block

Thing 1 | Thing 2 | 'Nuther Thing
--- | --- | ---
Goats | eat | mice
Lambs | eat | oats
Does | ee | doats
Little | lambs eat | iGoats

## Heading 1 Below Table

Code block that tests `font-variant-ligatures: none;` to properly render the string <span style={{fontVariantLigatures: "none", fontFeatureSettings: "'liga' 0"}}><code>value !‍‍‍‌= null</code></span>: 


**Include expression**: Optional JavaScript expression to specify fields to numerify. If empty (the default), the Function will attempt to numerify all fields – except those listed in **Ignore fields**, which takes precedence. Use the `name` and `value` global variables to access fields' names/values. (Example: <span style={{fontVariantLigatures: "none", fontFeatureSettings: "'liga' 0"}}><code>value !‍‍‍‌= null</code></span>.) You can access other fields' values via `__e.<fieldName>`.

Now that we've added `font-variant-ligatures: none;` to global CSS, this paragraph tests that `!=` renders properly with just backticks.

## HTML-Tagged Code Example

Here's a live example to copy:

<div class="code-no-ligatures">

{"time":"2021-11‑24T15:12:05.713Z","cid":"w1","channel":"server","level":"info",​"message":"_raw stats","inEvents":31237,"outEvents":44791,"inBytes":7820263,​"outBytes":14701001,"starttime":1637766660,"endtime":1637766720,"activeCxn":0,"openCxn":136,"closeCxn":135,"pqInEvents":0,"pqOutEvents":0,"pqInBytes":0,"pqOutBytes":0,"pqTotalBytes":0,"droppedEvents":1113,"activeEP":41,"blockedEP":5,"cpuPerc":23.34,"mem":{"heap":119,"ext":79,"rss":380}}

</div>

Below is how it's tagged. Note the hard returns between the tags and the code block (top and bottom) – without them, this won't compile in React. Also note the camelCase on `className`:

```html
<div className="code-no-ligatures">

<whatever>

</div>
```
