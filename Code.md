// This code includes the main game and excludes the game logic
#include <SFML/Graphics.hpp>
#include <SFML/Audio.hpp>
#include <iostream>
#include <string>

using namespace std;
using namespace sf;

Music playingMusic;

const string mainPagePath = "C:/Users/DELL/OneDrive/Desktop/Programs/Assets/Images/MainPage.jpg";
const string playPath = "C:/Users/DELL/OneDrive/Desktop/Programs/Assets/Images/Play.jpg";
const string settingsPath = "C:/Users/DELL/OneDrive/Desktop/Programs/Assets/Images/Settings.jpg";
const string progressPath = "C:/Users/DELL/OneDrive/Desktop/Programs/Assets/Images/Progress.jpg";
const string howToPlayPath = "C:/Users/DELL/OneDrive/Desktop/Programs/Assets/Images/HowToPlay.jpg";

const string mainMusicPath = "C:/Users/DELL/OneDrive/Desktop/Programs/Assets/Music/MainMusic.mp3";
const string secondaryMusicPath = "C:/Users/DELL/OneDrive/Desktop/Programs/Assets/Music/SecondMusic.mp3";

bool Clicked(const Event::MouseButtonEvent& mouseButton, const RectangleShape& rect) {
    FloatRect rectBounds = rect.getGlobalBounds();
    return rectBounds.contains(static_cast<float>(mouseButton.x), static_cast<float>(mouseButton.y));
}

void makingWindow(RenderWindow& window) {
    window.create(VideoMode(1000, 600), "The Wing Warriors");
}

void loadImage(Texture& texture, Sprite& sprite, const string& imagePath, int originX, int originY) {
    if (!texture.loadFromFile(imagePath)) {
        cout << "Error in showing image file: " << imagePath << endl;
        exit(0);
    }
    sprite.setTexture(texture);
    sprite.setPosition(originX, originY);
}

void playMusic(const string& musicPath) {
    if (!playingMusic.openFromFile(musicPath)) {
        cout << "Error in loading the music of : " << musicPath << endl;
        exit(0);
    }
    playingMusic.setLoop(true);
    playingMusic.play();
}

int main() {
    RenderWindow window;
    makingWindow(window);

    Texture mainTexture, playTexture, settingsTexture, progressTexture, howToPlayTexture;
    Sprite mainSprite, playSprite, settingsSprite, progressSprite, howToPlaySprite;

    loadImage(mainTexture, mainSprite, mainPagePath, 0, 0);
    loadImage(playTexture, playSprite, playPath, 0, 0);
    loadImage(settingsTexture, settingsSprite, settingsPath, 0, 0);
    loadImage(progressTexture, progressSprite, progressPath, 0, 0);
    loadImage(howToPlayTexture, howToPlaySprite, howToPlayPath, 0, 0);

    playMusic(mainMusicPath);

    RectangleShape playButton(Vector2f(300, 95));
    playButton.setPosition(650, 75);
    playButton.setFillColor(Color(255, 255, 255, 13));

    RectangleShape settingsButton(Vector2f(300, 95));
    settingsButton.setPosition(650, 220);
    settingsButton.setFillColor(Color(255, 255, 255, 13));

    RectangleShape quitButton(Vector2f(300, 95));
    quitButton.setPosition(650, 375);
    quitButton.setFillColor(Color(255, 255, 255, 13));

    RectangleShape progressButton(Vector2f(300, 100));
    progressButton.setPosition(620, 325);
    progressButton.setFillColor(Color(255, 255, 255, 13));

    RectangleShape howToPlayButton(Vector2f(300, 100));
    howToPlayButton.setPosition(620, 480);
    howToPlayButton.setFillColor(Color(255, 255, 255, 13));

    RectangleShape musicOnOffButton(Vector2f(300, 100));
    musicOnOffButton.setPosition(620, 175);
    musicOnOffButton.setFillColor(Color(255, 255, 255, 13));

    RectangleShape settingsBackButton(Vector2f(200, 55));
    settingsBackButton.setPosition(50, 20);
    settingsBackButton.setFillColor(Color(255, 255, 255, 13));

    RectangleShape playBackButton(Vector2f(180, 55));
    playBackButton.setPosition(30, 20);
    playBackButton.setFillColor(Color(255, 255, 255, 13));

    RectangleShape howToPlayBackButton(Vector2f(180, 55));
    howToPlayBackButton.setPosition(780, 520);
    howToPlayBackButton.setFillColor(Color(255, 255, 255, 13));

    RectangleShape progressBackButton(Vector2f(180, 55));
    progressBackButton.setPosition(780, 520);
    progressBackButton.setFillColor(Color(255, 255, 255, 13));


    bool mainPageVisible = true;
    bool settingsVisible = false;
    bool howToPlayVisible = false;
    bool progressVisible = false;
    bool playVisible = false;

    bool playButtonActive = true;
    bool quitButtonActive = true;
    bool settingsButtonActive = true;

    bool settingsBackButtonActive = false;
    bool howToPlayButtonActive = false;
    bool progressButtonActive = false;
    bool musicOnOffButtonActive = false;
    bool playBackButtonActive = false;
    bool  howToPlayBackButtonActive = false;
    bool progressBackButtonActive = false;
    bool musicIsPlaying = true;

    while (window.isOpen()) {
        Event event;
        while (window.pollEvent(event)) {
            if (event.type == Event::Closed) {
                window.close();
            }
            else if (event.type == Event::MouseButtonPressed && event.mouseButton.button == Mouse::Left) {
                if (mainPageVisible) {
                    if (Clicked(event.mouseButton, playButton) && playButtonActive) {
                        loadImage(playTexture, playSprite, playPath, 0, 0);
                        playMusic(secondaryMusicPath);
                        playVisible = true;
                        mainPageVisible = false;
                        settingsVisible = false;
                        playButtonActive = false;
                        quitButtonActive = false;
                        settingsButtonActive = false;
                        playBackButtonActive = true;
                    }
                    else if (Clicked(event.mouseButton, quitButton) && quitButtonActive) {
                        exit(0);
                    }
                    else if (Clicked(event.mouseButton, settingsButton) && settingsButtonActive) {
                        loadImage(settingsTexture, settingsSprite, settingsPath, 0, 0);
                        playMusic(mainMusicPath);
                        settingsVisible = true;
                        mainPageVisible = false;
                        playVisible = false;

                        playButtonActive = false;
                        quitButtonActive = false;
                        settingsButtonActive = false;

                        settingsBackButtonActive = true;
                        howToPlayButtonActive = true;
                        progressButtonActive = true;
                        musicOnOffButtonActive = true;
                    }
                }
                else if (playVisible) {
                    if (Clicked(event.mouseButton, playBackButton) && playBackButtonActive) {
                        loadImage(mainTexture, mainSprite, mainPagePath, 0, 0);
                        playMusic(mainMusicPath);
                        mainPageVisible = true;
                        settingsVisible = false;
                        howToPlayVisible = false;
                        progressVisible = false;
                        playVisible = false;

                        playButtonActive = true;
                        quitButtonActive = true;
                        settingsButtonActive = true;

                        settingsBackButtonActive = false;
                        howToPlayButtonActive = false;
                        progressButtonActive = false;
                        musicOnOffButtonActive = false;
                    }
                }
                else if (settingsVisible) {
                    if (Clicked(event.mouseButton, settingsBackButton) && settingsBackButtonActive) {
                        loadImage(mainTexture, mainSprite, mainPagePath, 0, 0);
                        playMusic(mainMusicPath);
                        mainPageVisible = true;
                        settingsVisible = false;
                        howToPlayVisible = false;
                        progressVisible = false;
                        playVisible = false;

                        playButtonActive = true;
                        quitButtonActive = true;
                        settingsButtonActive = true;

                        settingsBackButtonActive = false;
                        howToPlayButtonActive = false;
                        progressButtonActive = false;
                        musicOnOffButtonActive = false;
                    }
                    else if (Clicked(event.mouseButton, howToPlayButton) && howToPlayButtonActive) {
                        loadImage(howToPlayTexture, howToPlaySprite, howToPlayPath, 0, 0);
                        playMusic(mainMusicPath);
                        mainPageVisible = false;
                        settingsVisible = false;
                        howToPlayVisible = true;
                        progressVisible = false;
                        playVisible = false;

                        playButtonActive = false;
                        quitButtonActive = false;
                        settingsButtonActive = false;

                        settingsBackButtonActive = true;
                        howToPlayButtonActive = false;
                        progressButtonActive = false;
                        musicOnOffButtonActive = false;
                        howToPlayBackButtonActive = true;
                    }
                    else if (Clicked(event.mouseButton, progressButton) && progressButtonActive) {
                        loadImage(progressTexture, progressSprite, progressPath, 0, 0);
                        playMusic(mainMusicPath);
                        mainPageVisible = false;
                        settingsVisible = false;
                        howToPlayVisible = false;
                        progressVisible = true;
                        playVisible = false;

                        playButtonActive = false;
                        quitButtonActive = false;
                        settingsButtonActive = false;

                        settingsBackButtonActive = true;
                        howToPlayButtonActive = false;
                        progressButtonActive = false;
                        musicOnOffButtonActive = false;
                        progressBackButtonActive = true;
                    }
                    else if (Clicked(event.mouseButton, musicOnOffButton) && musicOnOffButtonActive) {
                        if (musicIsPlaying) {
                            playingMusic.pause();
                        }
                        else {
                            playingMusic.play();
                        }
                        musicIsPlaying = !musicIsPlaying;

                        settingsVisible = true;
                        mainPageVisible = false;
                        playVisible = false;

                        playButtonActive = false;
                        quitButtonActive = false;
                        settingsButtonActive = false;

                        settingsBackButtonActive = true;
                        howToPlayButtonActive = true;
                        progressButtonActive = true;
                        musicOnOffButtonActive = true;

                    }
                }
                else if (howToPlayVisible) {
                    if (Clicked(event.mouseButton, howToPlayBackButton) && howToPlayBackButtonActive) {
                        loadImage(settingsTexture, settingsSprite, settingsPath, 0, 0);
                        playMusic(mainMusicPath);
                        mainPageVisible = false;
                        settingsVisible = true;
                        howToPlayVisible = false;
                        progressVisible = false;
                        playVisible = false;

                        playButtonActive = false;
                        quitButtonActive = false;
                        settingsButtonActive = false;

                        settingsBackButtonActive = true;
                        howToPlayButtonActive = true;
                        progressButtonActive = true;
                        musicOnOffButtonActive = true;
                        playBackButtonActive = false;
                        howToPlayBackButtonActive = false;
                        progressBackButtonActive = false;
                        musicIsPlaying = true;
                    }
                }
                else if (progressVisible) {
                    if (Clicked(event.mouseButton, progressBackButton) && progressBackButtonActive) {
                        loadImage(settingsTexture, settingsSprite, settingsPath, 0, 0);
                        playMusic(mainMusicPath);
                        mainPageVisible = false;
                        settingsVisible = true;
                        howToPlayVisible = false;
                        progressVisible = false;
                        playVisible = false;

                        playButtonActive = false;
                        quitButtonActive = false;
                        settingsButtonActive = false;

                        settingsBackButtonActive = true;
                        howToPlayButtonActive = true;
                        progressButtonActive = true;
                        musicOnOffButtonActive = true;
                        playBackButtonActive = false;
                        howToPlayBackButtonActive = false;
                        progressBackButtonActive = false;
                        musicIsPlaying = true;
                    }
                }
            }
        }

        window.clear();

        if (mainPageVisible && !settingsVisible && !playVisible) {
            window.draw(mainSprite);

            if (playButtonActive) window.draw(playButton);
            if (settingsButtonActive) window.draw(settingsButton);
            if (quitButtonActive) window.draw(quitButton);
        }
        else if (settingsVisible) {
            window.draw(settingsSprite);
            if (settingsBackButtonActive) window.draw(settingsBackButton);
            if (howToPlayButtonActive) window.draw(howToPlayButton);
            if (musicOnOffButtonActive) window.draw(musicOnOffButton);
            if (progressButtonActive) window.draw(progressButton);
        }
        else if (playVisible && playBackButtonActive) {
            window.draw(playSprite);
            window.draw(playBackButton);
        }
        else if (howToPlayVisible && settingsBackButtonActive) {
            window.draw(howToPlaySprite);
            window.draw(howToPlayBackButton);
        }
        else if (progressVisible && settingsBackButtonActive) {
            window.draw(progressSprite);
            window.draw(progressBackButton);
        }

        window.display();
    }

    return 0;
}
