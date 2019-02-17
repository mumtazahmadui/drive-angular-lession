```
<html ng-app="photoSharingApp">
<head>
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">  
    <script type='text/javascript' src="https://code.angularjs.org/1.7.7/angular.min.js"></script>

  <style>
      body {
        padding: 50px;
        }

        input[type=text] {
        padding: 3px;
        border-radius: 5px;
        border: 1px solid #bbb;
        }

        .error {
        color: #a00;
        }

        textarea {
        padding: 3px;
        border-radius: 5px;
        border: 1px solid #bbb;
        }

        .album {
        width: 300px;
        float: left;
        margin-right: 10px;
        }

        div.album div.title {
        float: right;
        }
  </style>
</head>

<body ng-controller="AlbumListController">

  <p>
    <input type="text" placeholder="Search..." ng-model="searchFor" size="20"/>
  </p>

  <div class="album panel panel-primary" ng-repeat="album in albums | filter: { title: searchFor } ">
    <div class="panel-heading"> {{ album.title }} <div class="title" >{{ album.date }}</div> </div>
    <div class="panel-body">
      {{ album.description }}
    </div>
  </div>

  <div style="clear: left"></div>

  <h3> Add new album</h3>
  <div class="album panel panel-default">
    <div style="margin: 5px" class="alert alert-danger" ng-show="add_error_text">{{add_error_text}}</div>
    <form name="add_album_form">
      <div class="panel-heading">
        <input type="text" placeholder="Title..." size="20" ng-model="new_album.title"/>
        <div class="title" >
          <input type="text" placeholder="yyyy/mm/dd" size="10" 
                 ng-pattern="/[0-9]{4,4}[/\-. ,]+[0-9]{2,2}[/\-. ,]+[0-9]{2,2}/"
                 ng-model="new_album.date"/>
        </div>
      </div>
      <div class="panel-body">
        <p>
          <textarea ng-model="new_album.description" placeholder="Description..." rows='4' style="width: 100%"></textarea>
        </p>
        <p>
          <input type="text"
                 name="name"
                 ng-model="new_album.name"
                 placeholder="Short name for URL..."
                 style="width: 100%"
                 ng-minlength="6"
                 ng-maxlength="40"/>
        </p>
        <button ng-click="addAlbum(new_album)" type="button" class="btn btn-success">Add New Album</button>
      </div>
    </form>
  </div>



<script type="text/javascript">

var photoApp = angular.module("photoSharingApp", []);


function AlbumListController ($scope) {

    $scope.new_album = {};
    $scope.add_error_text = '';

    $scope.albums = [ 
        { name: 'madrid1309', title: 'Weekend in Madrid', date: '2013-09-01', description: 'My favourite trip' },
        { name: 'iceland1404', title: 'Holiday in Iceland', date: '2014-04-15', description: 'This place is cold' },
        { name: 'thailand1210', title: 'Surfing in Thailand', date: '2012-10-01', description: 'So hot!' },
        { name: 'australia1207', title: 'Wedding in Australia', date: '2012-07-31', description: 'So many kangaroos and koalas!' }
    ];

    $scope.addAlbum = function (album_data) {
        if (!album_data.title) 
            $scope.add_error_text = "Missing title";
        else if (!album_data.date)
            $scope.add_error_text = "You must specify a date (yyyy/mm/dd)";
        else if (!album_data.description)
            $scope.add_error_text = "Missing description";
        else if (!album_data.name)
            $scope.add_error_text = "Short album name must be at least 6 chars (ironic, yes)";
        else {
            try {
                var d = new Date(album_data.date.trim());
                if (isNaN(d.getTime())) throw new Error("invalid date");
                // if we made it here the date is good
                $scope.albums.push(album_data);
                $scope.new_album = {};
                $scope.add_error_text = '';
            } catch (e) {
                $scope.add_error_text = "You must specify a valid date (yyyy/mm/dd)";
            }
        }
    };
}

photoApp.controller("AlbumListController", AlbumListController);


</script>

</body>
</html>
```

