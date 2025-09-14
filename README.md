# 3D Flight Tracker - Enhanced

A comprehensive flight tracking system with both 2D and 3D visualization capabilities, integrating multiple data sources including FlightRadar24 and OpenSky Network.

## Features

### 🗺️ Dual View System
- **2D Map**: Traditional OpenLayers-based flight tracking with detailed markers and information
- **3D Map**: Google Maps 3D with Three.js aircraft models for immersive visualization

### ✈️ Aircraft Visualization
- **Real-time 3D Models**: Boeing F-18 and other aircraft models using GLTF format
- **Dynamic Positioning**: Aircraft positioned accurately based on real-time flight data
- **Category-based Styling**: Different colors and models for military, commercial, private, helicopter, and drone aircraft
- **Smooth Animations**: Real-time position updates with smooth transitions

### 📡 Data Sources
- **FlightRadar24**: Primary flight data source with comprehensive coverage
- **OpenSky Network**: Additional real-time flight data for enhanced coverage
- **CORS Proxy Support**: Automatic fallback for data fetching

### 🎮 Interactive Controls
- **Camera Modes**: Orbit, Follow Aircraft, and Free Camera
- **Model Scaling**: Adjustable aircraft model size
- **Flight Selection**: Click on aircraft to view details and follow in 3D
- **Group Filtering**: Organize flights by groups (A, B, C, etc.)

### 🎨 Visual Features
- **Altitude-based Coloring**: Aircraft colors change based on altitude
- **Heading-based Rotation**: Aircraft models rotate to show flight direction
- **Real-time Updates**: Automatic data refresh every 5 seconds
- **Responsive Design**: Works on desktop and mobile devices

## Usage

### Getting Started
1. Open `index.html` in a web browser
2. The system will automatically start fetching flight data
3. Use the control buttons to switch between 2D and 3D views

### Control Buttons
- **L**: Toggle between light and dark map themes
- **3D**: Switch between 2D and 3D visualization modes
- **OS**: Enable/disable OpenSky Network data
- **Menu**: Access detailed settings and information

### 3D Controls
- **Camera Mode**: Choose between Orbit, Follow Aircraft, or Free Camera
- **Model Scale**: Adjust the size of aircraft models
- **Reset Camera**: Return to default camera position
- **Toggle Models**: Show/hide aircraft models

### Flight Interaction
- **Click Aircraft**: View detailed flight information
- **Select Flight**: Choose aircraft to follow in 3D mode
- **View in 3D**: Switch to 3D view and follow selected aircraft

## Technical Details

### Technologies Used
- **Google Maps 3D**: For 3D terrain and satellite imagery
- **Three.js**: For 3D aircraft rendering and scene management
- **OpenLayers**: For 2D map visualization
- **Bootstrap**: For responsive UI components
- **GLTF Loader**: For 3D aircraft models

### API Keys
- **Google Maps API**: `AIzaSyA6myHzS10YXdcazAFalmXvDkrYCp5cLc8`
- **FlightRadar24**: Public API endpoints
- **OpenSky Network**: Public API endpoints

### File Structure
```
/workspace/
├── index.html              # Main application file
├── indexx.html             # Original 2D-only version
├── file3d3.html            # Original 3D test file
├── boeing-f18/             # Aircraft model directory
│   ├── scene.gltf          # Main aircraft model
│   ├── scene.bin           # Model binary data
│   └── textures/           # Model textures
└── README.md               # This file
```

### Performance Optimization
- **LOD System**: Different model complexity based on distance
- **Frustum Culling**: Only render visible aircraft
- **Instance Rendering**: Efficient rendering of multiple similar aircraft
- **Data Grouping**: Organize flights into manageable groups

## Browser Compatibility
- **Chrome**: Full support
- **Firefox**: Full support
- **Safari**: Full support
- **Edge**: Full support

## Requirements
- Modern web browser with WebGL support
- Internet connection for data fetching
- JavaScript enabled

## Troubleshooting

### Common Issues
1. **Models not loading**: Check browser console for CORS errors
2. **No flight data**: Verify internet connection and API availability
3. **Poor performance**: Reduce model scale or disable some features
4. **3D view not working**: Ensure WebGL is supported and enabled

### Performance Tips
- Use the group selector to limit displayed flights
- Adjust model scale for better performance
- Disable OpenSky data if not needed
- Close other browser tabs to free up memory

## Future Enhancements
- Additional aircraft models (helicopters, drones, etc.)
- Weather overlay integration
- Flight path prediction
- Multiplayer support for shared viewing
- Mobile app version
- Advanced filtering and search capabilities

## License
This project is for educational and demonstration purposes. Please respect the terms of service of the data providers (FlightRadar24, OpenSky Network, Google Maps).