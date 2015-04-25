# Add A Customized Controller And View

If you need to create a customized controller and view under panel, please read this section.

First create a controller and extend it from the Controller class (instead of the CrudController), for example if the controller is named TestCustomController :

	TestCustomController extends Controller {

		public function __construct() { }

		public function index()
		{
			// Code

			return view('test_custom_view');
		}
	}

Then you should create your view file, for example if your view is named test_custom_view, create the test_custom_view.blade.php file under the resources/views folder and to load templates of panel, you should include the following lines in your view file :

	@extends('panelViews::mainTemplate')
	@section('page-wrapper')

	<!-- Code -->

	@stop
