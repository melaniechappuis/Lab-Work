main.cpp

    #include "ofMain.h"
    #include "ofApp.h"

    int main( ){

    ofSetupOpenGL(1024,768, OF_WINDOW);           

    ofRunApp( new ofApp());

    }

ofApp.cpp

    #include "ofApp.h"
    
    void ofApp::setup(){
    
     ofEnableDepthTest();
    
    dim=100;
    spacing = ((PI * 2) / dim);
    size = 3.5;
    
    for (int i = 0; i < dim + 1; i++) {
  
    float z = size * cos(spacing / 2 * i);
  
    float s = size * sin(spacing / 2 * i);
    
    for (int j = 0; j < dim ; j++ ) {
    
     ofVec3f point;
            
            point.set(cos(spacing * j) * s,sin(spacing * j) * s,z);
            
            points.push_back(point);

        }
    }
    
    cout << "done";
    
     for (int i = 0; i < points.size(); i++) {
        
        cout << points[i].x << " " << points[i].y << " " << points[i].z << endl;
        
    }
  
    cam.setNearClip(1);
    cam.setFarClip(-1);
    cam.setPosition(0,0,10);
    
    glEnable(GL_LIGHTING);
    glEnable(GL_LIGHT0);
    
    GLfloat ambientLight[] = { 0.2f, 0.2f, 0.2f, 1.0f };
    GLfloat diffuseLight[] = { 0.8f, 0.8f, 0.2, 1.2f };
    GLfloat specularLight[] = { 1.1f, 0.15f, 0.05f, 0.4f };
    
    glLightfv(GL_LIGHT0, GL_AMBIENT, ambientLight);
    glLightfv(GL_LIGHT0, GL_DIFFUSE, diffuseLight);
    glLightfv(GL_LIGHT0, GL_SPECULAR, specularLight);
    
    glEnable(GL_COLOR_MATERIAL);
    
    glColorMaterial(GL_FRONT, GL_AMBIENT_AND_DIFFUSE);
    
    glShadeModel(GL_SMOOTH);
    
    void ofApp::update(){
    
    }
    
    ofVec3f ofApp::normal(std::vector<ofVec3f> tri) {
    
    ofVec3f a, b, normal;
    
    a.x = tri[0].x - tri[1].x;
    a.y = tri[0].y - tri[1].y;
    a.z = tri[0].z - tri[1].z;
    
    b.x = tri[1].x - tri[2].x;
    b.y = tri[1].y - tri[2].y;
    b.z = tri[1].z - tri[2].z;
    
    normal.x = (a.y * b.z) - (a.z * b.y);
    normal.y = (a.z * b.x) - (a.x * b.z);
    normal.z = (a.x * b.y) - (a.y * b.x);
    
    return normal.normalize();
    
    }
    
    void ofApp::draw(){
    
    ofBackground(0,0,225);
    
    cam.begin();
    
    ofRotateY(ofGetElapsedTimeMillis()/20);
    
    glBegin(GL_TRIANGLES);
    
    for (int i = dim; i < points.size(); i++) {
    
    std::vector<ofVec3f> triangle;
        ofVec3f vec;
        vec.set(points[i].x, points[i].y, points[i].z);
        triangle.push_back(vec);
        vec.set(points[i-1].x, points[i-1].y, points[i-1].z);
        triangle.push_back(vec);
        vec.set(points[i-dim].x, points[i-dim].y, points[i-dim].z);
        triangle.push_back(vec);
        
        vec = ofApp::normal(triangle);
        
        glNormal3f(vec.x, vec.y, vec.z);
        
        glVertex3f(points[i].x, points[i].y, points[i].z);
        glVertex3f(points[i-3].x, points[i-2].y, points[i-1].z);
        glVertex3f(points[i-dim].x, points[i-dim].y, points[i-dim].z);
        
        std::vector<ofVec3f> triangle2;
        
        if (dim >1) {
        
        vec.set(points[i-dim-1].x, points[i-dim-10].y, points[i-dim-1].z);
        triangle2.push_back(vec);
        
        } else {
        
        vec.set(points[i-dim].x, points[i-dim].y, points[i-dim].z);
        triangle2.push_back(vec);
        
        }
        
        vec.set(points[i-dim].x, points[i-dim].y, points[i-dim].z);
        triangle2.push_back(vec);
        vec.set(points[i-1].x, points[i-1].y, points[i-1].z);
        triangle2.push_back(vec);
        
        vec = ofApp::normal(triangle2);
        
        glNormal3f(vec.x, vec.y, vec.z);
        glVertex3f(points[i-dim-1].x, points[i-dim-1].y, points[i-dim-1].z);
        glVertex3f(points[i-dim].x, points[i-dim].y, points[i-dim].z);
        glVertex3f(points[i-1].x, points[i-1].y, points[i-1].z);
        
        }
        
        glEnd();

     cam.end();
     }
     
     void ofApp::keyPressed(int key){
     
     }
     
     void ofApp::keyReleased(int key){
     
     }
     
     void ofApp::mouseMoved(int x, int y){
     
        GLfloat position[] = { static_cast<GLfloat>((float) ofGetWidth()-mouseX), static_cast<GLfloat>((float) mouseY), 10.0f, 1.0f };
    glLightfv(GL_LIGHT0, GL_POSITION, position);

    }
    
    void ofApp::mouseDragged(int x, int y, int button){

    }
    
    void ofApp::mousePressed(int x, int y, int button){

    }
    
    void ofApp::mouseReleased(int x, int y, int button){

    }
    
    void ofApp::mouseEntered(int x, int y){

    }
    
    void ofApp::mouseExited(int x, int y){

    }
    
    void ofApp::windowResized(int w, int h){

    }
    
    void ofApp::gotMessage(ofMessage msg){

    }
    
    void ofApp::dragEvent(ofDragInfo dragInfo){

    }
    
    
 ofApp.h
 
    #pragma once

    #include "ofMain.h"

    class ofApp : public ofBaseApp{
    public:
        void setup();
        void update();
        void draw();
    
    ofVec3f normal(std::vector<ofVec3f> tri);
    
        void keyPressed(int key);
        void keyReleased(int key);
        void mouseMoved(int x, int y);
        void mouseDragged(int x, int y, int button);
        void mousePressed(int x, int y, int button);
        void mouseReleased(int x, int y, int button);
        void mouseEntered(int x, int y);
        void mouseExited(int x, int y);
        void windowResized(int w, int h);
        void dragEvent(ofDragInfo dragInfo);
        void gotMessage(ofMessage msg);
    
    
    int dim;
    float spacing;
    int size;
    
    ofCamera cam;
    
    std::vector<ofVec3f> points;
    
    ofMesh myMesh;
        
    };

 
 <img width="1204" alt="Screenshot 2021-04-12 at 15 24 53" src="https://user-images.githubusercontent.com/73337770/114410415-44223880-9ba3-11eb-8727-6181c0db6f67.png">

    
