# 欢迎来访，留个脚印吧~

<div id="gitalk-container"></div>

<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.css">
<script src="https://cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.min.js"></script>
<div id="gitalk-container"></div>
<script>
    var gitalk = new Gitalk({
      "clientID": "ebc418bb2f62d385ea9b",
      "clientSecret": "4ed34648983a156a67f51743c0c3a07704b4ecd8",
      "repo": "book",
      "owner": "LevisLv",
      "admin": ["LevisLv"],
      id: decodeURI(location.pathname),
      perPage: 20,
      createIssueManually: true
    });
    gitalk.render("gitalk-container");
</script>
