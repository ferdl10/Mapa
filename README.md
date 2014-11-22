Mapas

    public class Principal extends FragmentActivity implements LocationListener {

    LocationManager locationManager;
    private GoogleMap mMap; // Might be null if Google Play services APK is not available.
    Marker chihuahua;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_principal);
        setUpMapIfNeeded();

        mMap.setMapType(GoogleMap.MAP_TYPE_HYBRID);

        UiSettings config = mMap.getUiSettings();
        config.setRotateGesturesEnabled(false);
        config.setCompassEnabled(false);

        locationManager = (LocationManager) getSystemService(LOCATION_SERVICE);
        locationManager.requestLocationUpdates(LocationManager.GPS_PROVIDER,
                0, 0, (LocationListener) this);

    }

    @Override
    protected void onResume() {
        super.onResume();
        setUpMapIfNeeded();
    }
  
    private void setUpMapIfNeeded() {
        // Do a null check to confirm that we have not already instantiated the map.
        if (mMap == null) {
            // Try to obtain the map from the SupportMapFragment.
            mMap = ((SupportMapFragment) getSupportFragmentManager().findFragmentById(R.id.map))
                    .getMap();
            // Check if we were successful in obtaining the map.
            if (mMap != null) {
                setUpMap();
            }
        }
    }

    private void setUpMap() {
    }

    @Override
    public void onLocationChanged(Location location) {

        CameraUpdate cambiar = CameraUpdateFactory.newLatLng(new LatLng(location.getAltitude(), location.getLongitude()));
        mMap.moveCamera(cambiar);
        mMap.animateCamera(CameraUpdateFactory.newLatLngZoom(new LatLng(location.getAltitude(), location.getLongitude()),18));

        if (location.getLatitude() != 0.0 && location.getLongitude() != 0.0) {
            try {
                chihuahua = mMap.addMarker(new MarkerOptions().position(new LatLng(location.getAltitude(), location.getLongitude())).title("Marker"));
                chihuahua.setDraggable(true);
            }catch (Exception e){
            }
        }

    }

    @Override
    public void onStatusChanged(String s, int i, Bundle bundle) {
    }

    @Override
    public void onProviderEnabled(String s) {
        Toast.makeText(this, "El proveedor " + s + "se habilitó.", Toast.LENGTH_SHORT).show();
    }

    @Override
    public void onProviderDisabled(String s) {
        Toast.makeText(this, "El proveedor " + s + "se deshabilitó.", Toast.LENGTH_SHORT).show();
    }
    }
