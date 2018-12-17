## Index

[TOC]

- [Bootstrap](#bootstrap)

_____________________________________________________________________________________
Bootstrap
-------------------------------------------------------------------------------------

- [Documentation](http://getbootstrap.com/docs/4.1/getting-started/introduction/)
- [Cheat Sheet](https://hackerthemes.com/bootstrap-cheatsheet/)
- [Visual examples bootstrap 3](https://www.w3schools.com/bootstrap/)
- [Visual examples bootstrap 4](https://www.w3schools.com/bootstrap4/default.asp)
- [Material Design](https://mdbootstrap.com/docs/jquery/getting-started/download/)
- [Editor](https://www.bootply.com/)
- [Customize](https://getbootstrap.com/docs/3.3/customize/)
- [Templates](https://bootswatch.com/)

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- The above 3 meta tags *must* come first in the head; any other head content must come *after* these tags -->
    <meta name="description" content="">
    <meta name="author" content="">
    <link rel="icon" href="../../favicon.ico">

    <title>Starter Template for Bootstrap</title>

    <!-- Bootstrap core CSS -->
    <link href="../../dist/css/bootstrap.min.css" rel="stylesheet">

    <!-- IE10 viewport hack for Surface/desktop Windows 8 bug -->
    <link href="../../assets/css/ie10-viewport-bug-workaround.css" rel="stylesheet">

    <!-- Custom styles for this template -->
    <link href="starter-template.css" rel="stylesheet">

    <!-- Just for debugging purposes. Don't actually copy these 2 lines! -->
    <!--[if lt IE 9]><script src="../../assets/js/ie8-responsive-file-warning.js"></script><![endif]-->
    <script src="../../assets/js/ie-emulation-modes-warning.js"></script>

    <!-- HTML5 shim and Respond.js for IE8 support of HTML5 elements and media queries -->
    <!--[if lt IE 9]>
      <script src="https://oss.maxcdn.com/html5shiv/3.7.3/html5shiv.min.js"></script>
      <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
    <![endif]-->
  </head>

  <body>

    <nav class="navbar navbar-inverse navbar-fixed-top">
      <div class="container">
        <div class="navbar-header">
          <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar" aria-expanded="false" aria-controls="navbar">
            <span class="sr-only">Toggle navigation</span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
          </button>
          <a class="navbar-brand" href="#">Project name</a>
        </div>
        <div id="navbar" class="collapse navbar-collapse">
          <ul class="nav navbar-nav">
            <li class="active"><a href="#">Home</a></li>
            <li><a href="#about">About</a></li>
            <li><a href="#contact">Contact</a></li>
          </ul>
        </div><!--/.nav-collapse -->
      </div>
    </nav>

    <div class="container">

      <div class="starter-template">
        <h1>Bootstrap starter template</h1>
        <p class="lead">Use this document as a way to quickly start any new project.<br> All you get is this text and a mostly barebones HTML document.</p>
      </div>

    </div><!-- /.container -->


    <!-- Bootstrap core JavaScript
    ================================================== -->
    <!-- Placed at the end of the document so the pages load faster -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
    <script>window.jQuery || document.write('<script src="../../assets/js/vendor/jquery.min.js"><\/script>')</script>
    <script src="../../dist/js/bootstrap.min.js"></script>
    <!-- IE10 viewport hack for Surface/desktop Windows 8 bug -->
    <script src="../../assets/js/ie10-viewport-bug-workaround.js"></script>
  </body>
</html>
```

**CDN Bootstrap 3**

```html
<!-- Latest compiled and minified CSS -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
<!-- Optional theme -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap-theme.min.css" integrity="sha384-rHyoN1iRsVXV4nD0JutlnGaslCJuC7uwjduW9SVrLvRYooPp2bWYgmgJQIXwl/Sp" crossorigin="anonymous">
...
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<!-- Latest compiled and minified JavaScript -->
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js" integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

**CDN Bootstrap 4**

```html
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css" integrity="sha384-MCw98/SFnGE8fJT3GXwEOngsV7Zt27NXFoaoApmYm81iuXoPkFOJwJ8ERdknLPMO" crossorigin="anonymous">

<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.3/umd/popper.min.js" integrity="sha384-ZMP7rVo3mIykV+2+9J3UJ46jBk0WLaUAdn689aCwoqbBJiSnjAK/l8WvCWPIPm49" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js" integrity="sha384-ChfqqxuZUCnJSK3+MXmPNIyE6ZbWh2IMqE241rYiqJxyMiZ6OW/JmZQ5stwEULTy" crossorigin="anonymous"></script>
```

**Alerts**

    alert-primary
    alert-secondary
    alert-success
    alert-info
    alert-warning
    alert-danger
    alert-light
    alert-dark
    alert-link
    alert-dismissible
    alert-heading
_____________________________________________________________________________________
**Badges**

    badge
    badge-pill
    badge-primary
    badge-secondary
    badge-success
    badge-info
    badge-warning
    badge-danger
    badge-light
    badge-dark
_____________________________________________________________________________________
**Breadcrumbs**

    breadcrumb
_____________________________________________________________________________________
**Buttons**

    btn-primary
    btn-secondary
    btn-success
    btn-info
    btn-warning
    btn-danger
    btn-light
    btn-dark
    btn-link
    btn-outline-primary
    btn-outline-secondary
    btn-outline-success
    btn-outline-info
    btn-outline-warning
    btn-outline-danger
    btn-outline-light
    btn-outline-dark
_____________________________________________________________________________________
**Button Groups**

    btn-group
    btn-group-lg
    btn-group-sm
    btn-group-vertical
    btn-group (nested)
    btn-toolbar
_____________________________________________________________________________________
**Button Modifiers**

    btn-lg
    btn-sm
    btn-block
    active button
    disabled button
    checkbox as button
    radio as button
_____________________________________________________________________________________
**Cards**

    card
    card-body
    card-title
    card-subtitle
    card-text
    card-link
    card-img-top
    middle image
    card-img-bottom
    card-img-overlay
    list-group
    card-header
    h*.card-header
    card-footer
    card-group
    card-deck
    card-columns
    card bg-... text-...
_____________________________________________________________________________________
**Carousel**

    carousel slide
    carousel-fade
    carousel-indicators
    carousel-caption
_____________________________________________________________________________________
**Collapse**

    collapse
    accordion
_____________________________________________________________________________________
**Dropdowns**

    dropdown
    dropdown (split)
    dropup
    dropup (split)
    dropright
    dropleft
    dropdown-header
    dropdown-item-text
    dropdown-divider
    dropdown-item disabled
    dropdown-menu-right
_____________________________________________________________________________________
**Forms**

    form-group
    form-inline
    form using the grid
    form-control
    form-control-lg
    form-control-sm
    form-control-file
    form-control-plaintext
    form-control-range
    form-check
    form-check-inline
    disabled items
    readonly
_____________________________________________________________________________________
**Form Input Groups**

    input-group
    input-group-prepend
    input-group-append
    input-group-sm
    input-group-lg
    checkbox
    radio
    dropdown
    segmented buttons
_____________________________________________________________________________________
**Custom Forms**

    custom-checkbox
    custom-radio
    custom-select
    custom-file
    custom-range
_____________________________________________________________________________________
**Grid**

    container
    container-fluid
    row
    col-# ( <576px)
    col-sm-# ( ≥576px)
    col-md-# ( ≥768px)
    col-lg-# ( ≥992px)
    col-xl-# ( ≥1200px)
    col
    col-*
    no-gutters
    offset-*-#
    order-#
    nested columns
_____________________________________________________________________________________
**Images**

    img-fluid
    img-thumbnail
_____________________________________________________________________________________
**Jumbotron**

    jumbotron
    jumbotron-fluid
_____________________________________________________________________________________
**List Group**

    list-group
    list-group-item active
    list-group-item disabled
    list-group-item-action
    list-group-item-primary
    list-group-item-secondary
    list-group-item-success
    list-group-item-info
    list-group-item-warning
    list-group-item-danger
    list-group-item-light
    list-group-item-dark
    list-group with badges
    list-group with d-flex
_____________________________________________________________________________________
**Media Objects**

    media
    nested media
    d-flex align-self-start
    d-flex align-self-center
    d-flex align-self-end
    right aligned media
_____________________________________________________________________________________
**Modal**

    modal
    modal-dialog-centered
    modal fade
    modal-lg
    modal-sm
_____________________________________________________________________________________
**Navs**

    ul.nav
    nav.nav
    nav justify-content-*
    nav flex-column
    nav-tabs
    nav-pills
    nav-fill
    nav-justified
    nav with flex utils
    nav-tabs with dropdown
    nav-pills with dropdown
_____________________________________________________________________________________
**Navbar**

    navbar
    navbar-brand
    navbar with form
    navbar-text
    navbar-dark bg-dark
    navbar-light
    navbar fixed-top
    navbar fixed-bottom
    navbar sticky-top
    collapse navbar-collapse
    navbar-toggler
    navbar-expand-*
_____________________________________________________________________________________
**Pagination**

    pagination
    page-item disabled
    page-item active
    pagination-lg
    pagination-sm
_____________________________________________________________________________________
**Popover**

    popovers
    dismissible popover
_____________________________________________________________________________________
**Progress**

    progress
    progress-bar
    progress-bar with label
    progress-bar with height
    progress-bar bg-*
    multiple progress-bar
    progress-bar-striped
    progress-bar-striped bg-*
    progress-bar-animated
_____________________________________________________________________________________
**Scrollspy**

    data-spy
_____________________________________________________________________________________
**Tables**

    table
    thead-light
    thead-dark
    table-striped
    table-bordered
    table-borderless
    table-hover
    table-sm
    table-*-responsive
    table-reflow
    table-active
    table-primary
    table-secondary
    table-success
    table-info
    table-warning
    table-danger
    table-light
    table-dark
_____________________________________________________________________________________
**Tooltips**

    tooltip
_____________________________________________________________________________________
**Typography**

    display-# (1-4)
    lead
    blockquote
    blockquote-footer
    blockquote-reverse
    list-unstyled
    list-inline
    dl-horizontal
_____________________________________________________________________________________
**Utility: Borders**

    border
    border-*-0
    border-primary
    border-secondary
    border-success
    border-danger
    border-warning
    border-info
    border-light
    border-dark
    border-white
    rounded
    rounded-circle
    rounded-*
_____________________________________________________________________________________
**Utility: Colors**

    bg-primary
    bg-secondary
    bg-success
    bg-info
    bg-warning
    bg-danger
    bg-light
    bg-dark
    bg-white
    text-primary
    text-secondary
    text-success
    text-info
    text-warning
    text-danger
    text-light
    text-dark
    text-white
_____________________________________________________________________________________
**Utility: Display**

    d-*-block
    d-*-flex
    d-*-inline
    d-*-inline-block
    d-*-inline-flex
    d-*-none
    d-*-table
    d-*-table-cell
    d-print-...
_____________________________________________________________________________________
**Utility: Flexbox**

    flex-*-column
    flex-*-column-reverse
    flex-*-row
    flex-*-row-reverse
    flex-*-nowrap
    flex-*-wrap
    flex-*-wrap-reverse
    flex-fill
    flex-*-grow-1
    flex-*-grow-0
    flex-*-shrink-1
    flex-*-shrink-0
    justify-content-*-start
    justify-content-*-end
    justify-content-*-center
    justify-content-*-between
    justify-content-*-around
    align-content-*-start
    align-content-*-end
    align-content-*-center
    align-content-*-around
    align-content-*-stretch
    align-items-*-start
    align-items-*-end
    align-items-*-center
    align-items-*-baseline
    align-items-*-stretch
    align-self-*-start
    align-self-*-end
    align-self-*-center
    align-self-*-baseline
    align-self-*-stretch
    order-*-#
_____________________________________________________________________________________
**Utility: Misc**

    close
    embed-responsive
    shadow-none
    shadow-sm
    shadow
    shadow-lg
    invisible
    visible
    sr-only
    sr-only-focusable
_____________________________________________________________________________________
**Utility: Positioning**

    align-*
    clearfix
    fixed-top
    fixed-bottom
    sticky-top
    float-*-left
    float-*-right
    float-*-none
_____________________________________________________________________________________
**Utility: Sizing**

    w-100
    h-100
    mw-100
    mh-100
_____________________________________________________________________________________
**Utility: Spacing**

    m-1 / m-*-#
    mt-1 / mt-*-#
    mr-1 / mr-*-#
    mb-1 / mb-*-#
    ml-1 / ml-*-#
    mx-1 / mx-*-#
    my-1 / my-*-#
    p-1 / p-*-#
    pt-1 / pt-*-#
    pr-1 / pr-*-#
    pb-1 / pb-*-#
    pl-1 / pl-*-#
    px-1 / px-*-#
    py-1 / py-*-#
_____________________________________________________________________________________
**Utility: Text**

    font-weight-bold
    font-weight-light
    font-weight-normal
    font-italic
    text-monospace
    text-justify
    text-nowrap
    text-*-left
    text-*-right
    text-*-center
    text-lowercase
    text-uppercase
    text-capitalize
    text-truncate
    text-body
    text-black-50
    text-white-50
    text-muted
    text-hide

**[`^        back to top        ^`](#)**
