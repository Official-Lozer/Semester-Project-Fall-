#include <SFML/Graphics.hpp>
#include <iostream>
#include <vector>


using namespace std;
using namespace sf;

const string birdPath = "C:/Users/DELL/OneDrive/Desktop/Programs/Assets/Images/bird.jpg";
const string obstaclePath = "C:/Users/DELL/OneDrive/Desktop/Programs/Assets/Images/obstacle.jpg";

void loadImage(Texture& texture, Sprite& sprite, const string& imagePath, int originX, int originY) {
	if (!texture.loadFromFile(imagePath)) {
		cout << "Error in showing image file: " << imagePath << endl;
		exit(0);
	}
	sprite.setTexture(texture);
	sprite.setPosition(originX, originY);
}

void resizeSprite(Sprite& sprite, float scaleX, float scaleY) {
	sprite.setScale(scaleX, scaleY);
}

bool birdIsClicked = false;
bool birdIsLaunching = false;
int birdRegenerations;
bool obstacleIsKilled = false;

Vector2f birdPosition(30, 100);     //Initial positions
Vector2f launchVelocity;
vector<Vector2f> obstaclePositions(5);
vector<Sprite> obstacleSprites(5);
vector<bool> isObstacleDestroyed(5);

Texture birdTexture, obstacleTexture;
Sprite birdSprite, obstacleSprite;

void resetGame() {
    birdPosition = Vector2f(30, 100);
    loadImage(birdTexture, birdSprite, birdPath, birdPosition.x, birdPosition.y);
    birdIsClicked = false;
    birdIsLaunching = false;
    birdRegenerations = 3;

    //Resizing the bird sprite
    resizeSprite(birdSprite, 0.1, 0.1);

    for (int i = 0; i < 5; ++i) {
        obstaclePositions[i] = Vector2f(600, 50 + i * 100);
        loadImage(obstacleTexture, obstacleSprites[i], obstaclePath, obstaclePositions[i].x, obstaclePositions[i].y);
        isObstacleDestroyed[i] = false;

       
        resizeSprite(obstacleSprites[i], 0.5, 0.5);
    }
}

int main() {
	resetGame();

	Vector2f launchStartPosition;
	RenderWindow window(sf::VideoMode(800, 600), "The Wing Warriors");

    while (window.isOpen()) {
        Event event;
        while (window.pollEvent(event)) {
            if (event.type == Event::Closed) {
                window.close();
            }
            else if (event.type == Event::MouseButtonPressed && event.mouseButton.button == Mouse::Left) {
                if (!birdIsLaunching && birdRegenerations > 0) {
                    birdIsClicked = true;
                    launchStartPosition = static_cast<sf::Vector2f>(Mouse::getPosition(window));
                }
            }
            else if (event.type == Event::MouseButtonReleased && event.mouseButton.button == Mouse::Left) {
                if (birdIsClicked && birdRegenerations > 0) {
                    birdIsClicked = false;
                    Vector2f launchEndPosition = static_cast<Vector2f>(Mouse::getPosition(window));
                    launchVelocity = launchStartPosition - launchEndPosition;
                    birdIsLaunching = true;
                    birdRegenerations--;
                }
            }
        }

        if (birdIsClicked) {
                        Vector2f currentMousePosition = static_cast<sf::Vector2f>(Mouse::getPosition(window));
                        float distance = sqrt(pow(currentMousePosition.x - birdPosition.x, 2) +
                            pow(currentMousePosition.y - birdPosition.y, 2));
                        float angle = atan2(currentMousePosition.y - birdPosition.y, currentMousePosition.x - birdPosition.x);
                        float radius = min(100.0f, distance);
                        birdSprite.setPosition(birdPosition + Vector2f(std::cos(angle) * radius, std::sin(angle) * radius));
                    }
        else if (birdIsLaunching) {
            birdPosition += launchVelocity * 0.01f;
            birdSprite.setPosition(birdPosition);

            if (birdPosition.x < 0 || birdPosition.y < 0 || birdPosition.x > window.getSize().x ||
                birdPosition.y > window.getSize().y) {
                birdIsLaunching = false;

                if (birdRegenerations > 0) {
                    birdPosition = Vector2f(30, 300);
                    birdSprite.setPosition(birdPosition);
                }
            }
        }

        int remainingObstacles = 0;
        for (size_t i = 0; i < obstacleSprites.size(); ++i) {
            if (!isObstacleDestroyed[i]) {
                remainingObstacles++;
                if (birdSprite.getGlobalBounds().intersects(obstacleSprites[i].getGlobalBounds())) {
                    isObstacleDestroyed[i] = true;
                    cout << "Obstacle hit! You killed a monster!"<<endl;

                    if (!birdIsLaunching && birdRegenerations > 0) {
                        birdPosition = Vector2f(30.0f, 300.0f);
                        birdSprite.setPosition(birdPosition);
                        birdIsLaunching = false;
                    }
                }
            }
        }

        window.clear();
        window.draw(birdSprite);

        for (size_t i = 0; i < obstacleSprites.size(); ++i) {
            if (!isObstacleDestroyed[i]) {
                window.draw(obstacleSprites[i]);
            }
        }

        window.display();

        if (remainingObstacles == 0) {
            cout << "You win!"<<endl;
            cout << "Do you want to replay (r) or proceed to the next level (n)? ";
            char choice;
            cin >> choice;

            if (choice == 'r') {
                resetGame();
            }
            else if (choice == 'n') {
                cout << "You proceed to the next level!"<<endl;
                window.close();
            }
        }
        else if (remainingObstacles > 0 && birdRegenerations == 0) {
            cout << "You lose!"<<endl;
            cout << "Do you want to retry (y/n)? ";
            char choice;
            cin >> choice;

            if (choice == 'y') {
                resetGame();
            }
            else {
                window.close();
            }
        }
    }

    return 0;
}

