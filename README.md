<html>
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script>
    <style>
      .passed { color: #39aa56; }
      .failed { color: #db4545; }
    </style>
  </head>
  <body>
    <h1>Score <span id="score">0</span>/<span id="max-score"></span></h1>
    <ul id="results">
    </ul>
    
    <script>
      var player = 'ericminio';      
      var levels = ['thecodinggame-javascript-1', 'thecodinggame-javascript-2'];       
      var results = [];
      var calls = [];
      
      var api = 'https://api.travis-ci.org/repos/' + player + '/';
      $.each(levels, function(index, repo) {
        var target = api + repo + '.json';        
        var call = $.getJSON(target, function(data) { results[index] = (1-data.last_build_result); });
        calls.push(call);
      });
      
      var web = 'https://travis-ci.org/' + player + '/';
      $('#max-score').text(levels.length);          
      $.when.apply($, calls).done(function() {
        var score = 0;            
        $.each(levels, function(index, repo) {
          var result = results[index];          
          var href = web + repo;
          var color = result == 1 ? 'passed' : 'failed';
          var item = '<li class="'+ color +'"><a class="'+ color +'" href="'+ href +'">'+ repo +'</a></li>';
          
          $('#results').append(item);
          score += result;
        });
        $('#score').text(score);
      });
    </script>
  </body>
</html>
