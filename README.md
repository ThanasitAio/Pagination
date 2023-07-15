# Pagination

## Pagination Page

#### CSS
```css
li {
    cursor: pointer;
    color:#0000ff;
}

.NoDrop{
    cursor: no-drop;
}
```
#### HTML
```html
<div id="getTable">
        
 </div>
```

#### Javascript
```javascript

getTable('1')
function goToPage(page) {
    getTable(page)
    return false
}

function getTable(page){
    var dataSet = {"page":page}
    $.ajax({
        url : "pagetable.php",
        type: "POST",
        data: dataSet,
        success:function(result){
            document.getElementById('getTable').innerHTML = result;	
        }
    });
}
```


## Pagination Table
```php
$postPage = trim($_POST['page']);
$limit = 5;
$page = (isset($postPage) && is_numeric($postPage) ) ? $postPage : 1;
$paginationStart = ($page - 1) * $limit;
$authors = $db->query("SELECT * FROM authors LIMIT $paginationStart, $limit")->fetchAll();
// Get total records
$sql = $db->query("SELECT count(id) AS id FROM authors")->fetchAll();
$allRecrods = $sql[0]['id'];

// Calculate total pages
$totoalPages = ceil($allRecrods / $limit);
// Prev + Next
$prev = $page - 1;
$next = $page + 1;
```

### Pagination Table Data
```html
<div style="width:100%;height:40vh;overflow:auto;">
  <table class="table table-bordered mb-5">
      <thead>
          <tr class="table-success">
              <th scope="col"></th>
              <th scope="col"></th>
              <th scope="col"></th>
              <th scope="col"></th>
              <th scope="col"></th>
          </tr>
      </thead>
      <tbody>
          <?php foreach($Data as $valData): ?>
          <tr>
              <th scope="row"></th>
              <td></td>
              <td></td>
              <td>></td>
              <td></td>
          </tr>
          <?php ?>
      </tbody>
  </table>
</div>
```

### Pagination 
```html
<!-- Pagination -->
<div style="position: relative;">
  <div style="position: absolute; top: 5px; right: 0; bottom: 0;">
      <nav aria-label="Page navigation example mt-5">
          <ul class="pagination justify-content-center">
              <li class="page-item  <?php if ($page <= 1) { echo 'disabled NoDrop'; } ?>">
                  <a class="page-link <?php if ($page <= 1) { echo 'disabled-link'; } ?>" onclick="<?php if ($page <= 1) { echo 'javascript:void(0)'; } else { echo "goToPage(1)"; } ?>">First</a>
              </li>
              <li class="page-item <?php if ($page <= 1) { echo 'disabled NoDrop'; } ?>">
                  <a class="page-link" aria-label="Previous" onclick="<?php if ($page <= 1) { echo 'javascript:void(0)'; } else { echo "goToPage(" . $prev . ")"; } ?>" <?php if ($page <= 1) { echo 'onclick="return false;"'; } ?>>
                      <span aria-hidden="true">&laquo;</span>
                  </a>
              </li>

              <?php
              $visiblePages = 5; // Number of visible page links
              $halfVisible = floor($visiblePages / 2); // Half of the visible pages
              $startPage = max(1, $page - $halfVisible); // Calculate the starting page number
              $endPage = min($startPage + $visiblePages - 1, $totoalPages); // Calculate the ending page number
              // Adjust the starting page number if the ending page number is at the maximum limit
              $startPage = max(1, $endPage - $visiblePages + 1);
              // Display "..." if there are more pages before the visible range
              if ($startPage > 1) {
                  ?>
                  <li class="page-item">
                      <a class="page-link" onclick="goToPage(<?php echo $startPage - 1; ?>)">...</a>
                  </li>
                  <?php
              }
              for ($i = $startPage; $i <= $endPage; $i++) {
                  ?>
                  <li class="page-item <?php if ($page == $i) {
                      echo 'active';
                  } ?>">
                      <a class="page-link" onclick="goToPage(<?php echo $i; ?>)"> <?= $i; ?> </a>
                  </li>
                  <?php
              }
              // Display "..." if there are more pages after the visible range
              if ($endPage < $totoalPages) {
                  ?>
                  <li class="page-item">
                      <a class="page-link" onclick="goToPage(<?php echo $endPage + 1; ?>)">...</a>
                  </li>
                  <?php
              }
              ?>
              <li class="page-item <?php if ($page >= $totoalPages) { echo 'disabled NoDrop'; } ?>">
                  <a class="page-link" aria-label="Next" onclick="<?php if ($page >= $totoalPages) { echo 'javascript:void(0)'; } else { echo "goToPage(" . $next . ")"; } ?>" <?php if ($page >= $totoalPages) { echo 'onclick="return false;"'; } ?>>
                      <span aria-hidden="true">&raquo;</span>
                  </a>
              </li>
              <li class="page-item <?php if ($page >= $totoalPages) { echo 'disabled NoDrop'; } ?>">
                  <a class="page-link <?php if ($page >= $totoalPages) { echo 'disabled-link'; } ?>" onclick="<?php if ($page >= $totoalPages) { echo 'javascript:void(0)'; } else { echo "goToPage(" . $totoalPages . ")"; } ?>">Last</a>
              </li>
          </ul>
      </nav>
  </div>
</div>
```
