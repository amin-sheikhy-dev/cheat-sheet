```html
<html lang="en" dir="ltr">
  <!--* جهت سایت رو مشخص میکنه -->
  <head>
    <meta charset="UTF-8" />
    <meta name="description" content="dalam bache ha" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <!-- تایتل داکیومنت -->
    <title>HTML</title>

    <!-- لینک کردن ایکون -->
    <link rel="icon" type="image/jpg" href="./files/icon.png" />

    <!-- CSS لینک کردن -->
    <link rel="stylesheet" href="./" />
  </head>

  <style>
    * {
      font-size: 20px;
      background-color: #3d3d3d;
      margin-top: 20px;
      margin-bottom: 20px;
      border-radius: 10px;
      font-family: sans-serif, Verdana;
      padding-top: 4px;
      padding-bottom: 4px;
    }
  </style>

  <body>
    <!--* i -->
    <i>i tag</i>

    <!--* br -->
    <hr />

    <!--* strong -->
    <strong>strong tag</strong>

    <!--* hr -->
    <hr />

    <!--* del -->
    <del>del tag</del>

    <hr />

    <p>
      text
      <!--* sub sup -->
      <sub>sub tag</sub>
      <sup>sup tag</sup>
    </p>

    <hr />

    <!--* a -->
    <a href="#"><b># = khali - link tag</b></a>

    <hr />

    <!--* img -->
    <img src="./files/img-1.jpg" alt="html-alt" />

    <hr />

    <!--* picture -->
    <picture>
      <!-- اگه کمتر از 1000 پیکسل شد برو عکس بعدی -->
      <source media="(min-width:1000px)" src="./files/img-2.jpg" />

      <!-- اگه کمتر از 600 پیکسل شد برو عکس بعدی -->
      <source media="(min-width:600px)" src="./files/img-1.jpg" />

      <!-- اگه کمتر از 600 شد دیگه گزینه اخر اینه -->
      <img src="./files/icon.png" alt="man matnam" />
    </picture>

    <hr />

    <!--* table -->
    <table>
      <caption>karname man</caption>

      <tr>
        <th>dars</th>
        <th>nomre</th>
        <th>ostad</th>
      </tr>

      <tr>
        <td>gosaste</td>
        <td>18</td>
        <td>ehrahimi</td>
      </tr>

      <tr>
        <td>fizik</td>
        <td>15</td>
        <td>ghorbani</td>
      </tr>

      <tr>
        <td>madar</td>
        <td>13</td>
        <td>taheri asl</td>
      </tr>
    </table>

    <hr />

    <!------------------------------------------------------------------------------------------- table & colspan -->
    <table>
      <tr>
        <th colspan="2">name</th>
        <th>shomare</th>
      </tr>

      <tr>
        <td>amir</td>
        <td>javad</td>
        <td>802320</td>
      </tr>

      <tr>
        <td>hassan</td>
        <td>reza</td>
        <td>87239</td>
      </tr>

      <tr>
        <td>mohsen</td>
        <td>matin</td>
        <td>9379238</td>
      </tr>
    </table>

    <hr />

    <!--* List , ul -->
    <ul>
      <li>test</li>
      <li>test</li>
      <li>test</li>
      <li>test</li>
      <li>test</li>
      <li>test</li>
      <li>test</li>
    </ul>

    <hr />

    <!--* List , ol -->
    <ol type="A" start="6">
      <li>test</li>
      <li>test</li>
      <li>test</li>
      <li>test</li>
      <li>test</li>
      <li>test</li>
      <li>test</li>
      <li>test</li>
    </ol>

    <hr />

    <ol type="1" start="6">
      <li>test</li>
      <li>test</li>
      <li>test</li>
      <li>test</li>
      <li>test</li>
      <li>test</li>
      <li>test</li>
      <li>test</li>
    </ol>

    <hr />

    <!--* span -->
    <span>span tag</span>

    <hr />

    <!--* time -->
    <time datetime="2025">time tag</time>

    <hr />

    <!--* details -->
    <details>
      <summary>Lorem ipsum dolor sit amet.</summary>

      Lorem ipsum dolor sit amet consectetur, adipisicing elit. Facilis, recusandae fuga! Rem pariatur minima fugit sed. Tempore, deleniti similique
      commodi omnis perferendis itaque nesciunt minus voluptas eveniet, esse delectus aliquam?
    </details>

    <hr />

    <!------------------------------------------------------------------------------------------- details , open -->
    <details open>
      <summary>Lorem ipsum dolor sit amet.</summary>

      Lorem ipsum dolor sit amet consectetur, adipisicing elit. Facilis, recusandae fuga! Rem pariatur minima fugit sed. Tempore, deleniti similique
      commodi omnis perferendis itaque nesciunt minus voluptas eveniet, esse delectus aliquam?
    </details>

    <hr />

    <!--* pre -->
    <pre>
                      Lorem ipsum

    dolor sit amet

            consectetur adicing 
    </pre>

    <hr />

    <!--* kbd -->
    <kbd>ctrl+c</kbd>

    <hr />

    <!--* progress -->
    <progress max="10" value="4"></progress>

    <hr />

    <!--* fieldset -->
    <fieldset>
      <!--* legend -->
      <legend>FORM</legend>

      <!--* form -->
      <form action="" autocomplete="on">
        <!--* input , label -->

        <!------------------------------------------------------------------------------------------- text -->
        <label>(text)</label>
        <input type="text" name="name" value="" />

        <!------------------------------------------------------------------------------------------- number -->
        <label>(number)</label>
        <input type="number" name="age" value="" />

        <!------------------------------------------------------------------------------------------- radio -->
        <!-- باید یکی باشه مگرنه کاربر میتونه بیشتر از یک گزینه کلیک کنه name دقت کن که پراپرتی -->
        <div>
          <span>(radio)</span>

          <label>men</label>
          <input type="radio" name="gender" value="men" />

          <label>women</label>
          <input type="radio" name="gender" value="women" />
        </div>

        <!------------------------------------------------------------------------------------------- check box -->
        <div>
          <span>(check box)</span>

          <label>JavaScript</label>
          <input type="checkbox" name="skill1" value="JS" />

          <label>Python</label>
          <input type="checkbox" name="skill2" value="PY" />

          <label>PHP</label>
          <input type="checkbox" name="skill3" value="PHP" />
        </div>

        <!------------------------------------------------------------------------------------------- select box -->
        <label>(select box)</label>

        <select>
          <option value="riazi">riazi</option>
          <option value="fizi">fizi</option>
          <option value="madaar">madaar</option>
        </select>

        <!------------------------------------------------------------------------------------------- textarea -->
        <label>(textarea)</label>
        <textarea name="bio" value=""></textarea>

        <!------------------------------------------------------------------------------------------- search box -->
        <div>
          <label>(search box)</label>
          <input type="text" list="s.b" name="shahr" />
          <!-- یادت باشه حتما ایدی داشته باشه و لیست اینپوت رو وصلش کن به ایدی -->
          <datalist id="s.b">
            <option value="qom">qom</option>
            <option value="tehran">tehran</option>
            <option value="mashhad">mashhad</option>
            <option value="esfahan">esfahan</option>
            <option value="shiraz">shiraz</option>
            <option value="karaj">karaj</option>
          </datalist>
        </div>

        <!------------------------------------------------------------------------------------------- color -->
        <label>(color)</label>
        <input type="color" name="color" value="" />

        <!------------------------------------------------------------------------------------------- date -->
        <label>(date)</label>
        <input type="date" name="date" value="" />

        <!------------------------------------------------------------------------------------------- email -->
        <label>(email)</label>
        <input type="email" name="email" value="" />

        <!------------------------------------------------------------------------------------------- file -->
        <label>(file)</label>
        <input type="file" name="file" value="" />

        <!------------------------------------------------------------------------------------------- hidden type-->
        <label>(Hidden)</label>
        <input type="hidden" name="amin" value="hidden" />

        <!------------------------------------------------------------------------------------------- password -->
        <label>(password)</label>
        <input type="password" name="password" value="" />

        <!------------------------------------------------------------------------------------------- rage -->
        <label>(range)</label>
        <input type="range" name="" min="1" max="10" value="" />

        <!------------------------------------------------------------------------------------------- submit -->
        <!------------------------------------------------------------------------------------------- reset -->
        <input type="submit" value="" />
        <input type="reset" value="" />

        <!------------------------------------------------------------------------------------------- button -->
        <button type="submit">submit</button>
        <button type="reset">reset</button>
      </form>
    </fieldset>

    <fieldset>
      <legend>Attribiuts FORM</legend>
      <form action="">
        <!------------------------------------------------------------------------------------------- placeholder -->
        <input type="text" placeholder="your text...." />

        <!------------------------------------------------------------------------------------------- required -->
        <input type="text" required />

        <!------------------------------------------------------------------------------------------- autofocus -->
        <input type="text" autofocus />

        <!------------------------------------------------------------------------------------------- readonly -->
        <input type="text" readonly />

        <!------------------------------------------------------------------------------------------- min-max -->
        <input type="number" min="10" max="20" />

        <!------------------------------------------------------------------------------------------- disabled -->
        <button disabled>disabled</button>
        <input type="text" disabled />

        <!------------------------------------------------------------------------------------------- maxlength -->
        <input type="text" maxlength="8" />

        <!------------------------------------------------------------------------------------------- multiple -->
        <input type="file" multiple />
      </form>
    </fieldset>

    <hr />

    <!--* ifraim -->
    <!-- سایت رو بهت نشون میده -->
    <iframe src="https://vadana43.ec.iau.ir/p/4041/login/index.php"></iframe>

    <hr />

    <!-- هر کدوم ازینا 2 تا اتریبیوت میوتد و اتوپلی هم داره که من ننوشتم -->
    <!--* video -->
    <video src="./files/tom-jery.mp4" width="500px" controls></video>

    <hr />

    <!--* audio -->
    <audio src="./files/Billie Eilish - CHIHIRO.mp3" controls></audio>

    <audio src="./files/Shode peydat - young sudden.mp3" controls></audio>
  </body>
</html>
```
