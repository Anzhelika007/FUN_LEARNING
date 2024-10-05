<div>
  <button onclick="openTab(event, 'Tab1')">Tab 1</button>
  <button onclick="openTab(event, 'Tab2')">Tab 2</button>
</div>
<div id="Tab1" class="tabcontent">
  <h3>Tab 1</h3>
  <p>Содержимое первой вкладки.</p>
</div>
<div id="Tab2" class="tabcontent" style="display:none">
  <h3>Tab 2</h3>
  <p>Содержимое второй вкладки.</p>
</div>
<script>
function openTab(evt, tabName) {
  var i, tabcontent, tablinks;
  tabcontent = document.getElementsByClassName("tabcontent");
  for (i = 0; i < tabcontent.length; i++) {
    tabcontent[i].style.display = "none";
  }
  tablinks = document.getElementsByTagName("button");
  for (i = 0; i < tablinks.length; i++) {
    tablinks[i].className = tablinks[i].className.replace(" active", "");
  }
  document.getElementById(tabName).style.display = "block";
  evt.currentTarget.className += " active";
}
</script>
