#include <SFML/Graphics.hpp>
#include <iostream>
using namespace std;
int checkwin(int board[3][3])
{
    // row
    for (int row = 0; row < 3; row++)
    {
        if (board[row][0] != 0 && board[row][0] == board[row][1] && board[row][1] == board[row][2])
        {
            return board[row][0];
        }
    }
    // col
    for (int col = 0; col < 3; col++)
    {
        if (board[0][col] != 0 && board[0][col] == board[1][col] && board[1][col] == board[2][col])
        {
            return board[0][col];
        }
    }
    // diganal
    // primary diodonal
    if (board[0][0] != 0 && board[0][0] == board[1][1] && board[1][1] == board[2][2])
    {
        return board[0][0];
    }
    // secondary digonal
    if (board[0][2] != 0 && board[0][2] == board[1][1] && board[1][1] == board[2][0])
    {
        return board[0][2];
    }
    return 0;
}
int main()
{
    sf::RenderWindow window(sf::VideoMode(600, 600), "This is my 14th try");
    sf::RectangleShape v1(sf::Vector2f(5, 600));
    sf::RectangleShape v2(sf::Vector2f(5, 600));
    sf::RectangleShape h1(sf::Vector2f(600, 5));
    sf::RectangleShape h2(sf::Vector2f(600, 5));
    v1.setFillColor(sf::Color::Black);
    v2.setFillColor(sf::Color::Black);
    h1.setFillColor(sf::Color::Black);
    h2.setFillColor(sf::Color::Black);
    v1.setPosition(200, 0);
    v2.setPosition(400, 0);
    h1.setPosition(0, 200);
    h2.setPosition(0, 400);
    int board[3][3] = {0};
    bool xfirst = true;
    int winner = 0;
    // text
    sf::Font font;
    if (!font.loadFromFile("DejaVuSerif.ttf"))
    {
        std::cout << " not abale to find the font file" << std::endl;
        return -1;
    }
    sf::Text wintext;
    wintext.setFont(font);
    wintext.setFillColor(sf::Color::Green);
    wintext.setCharacterSize(50);
    wintext.setPosition(200, 100);

    sf::RectangleShape banner(sf::Vector2f(400, 250));
    banner.setPosition(100, 200);
    while (window.isOpen())
    {
        sf::Event event;
        while (window.pollEvent(event))
        {
            if (event.type == sf::Event::Closed)
            {
                window.close();
            }
            if (event.type == sf::Event::MouseButtonPressed && event.mouseButton.button == sf::Mouse::Left)
            {
                int x = event.mouseButton.x / 200;
                int y = event.mouseButton.y / 200;
                if (x >= 0 && x < 3 && y >= 0 && y < 3)
                {
                    if (board[y][x] == 0)
                    {
                        board[y][x] = xfirst ? 1 : 2;
                        xfirst = !xfirst;
                    }
                    winner = checkwin(board);
                }
            }
            if (event.type == sf ::Event::MouseButtonPressed && event.mouseButton.button == sf::Mouse::Right)
            {
                for (int row = 0; row < 3; row++)
                {
                    for (int col = 0; col < 3; col++)
                    {
                        board[row][col] = 0;
                    }
                }
                winner = 0;
                xfirst = true;
            }
        }
        window.clear(sf::Color::White);
        window.draw(v1);
        window.draw(v2);
        window.draw(h1);
        window.draw(h2);
        for (int row = 0; row < 3; row++)
        {
            for (int col = 0; col < 3; col++)
            {
                int cell = board[row][col];
                int cellx = col * 200;
                int celly = row * 200;
                if (cell == 1)
                {

                    sf::RectangleShape l1(sf::Vector2f(180, 5));
                    sf::RectangleShape l2(sf::Vector2f(180, 5));
                    l1.setFillColor(sf::Color::Red);
                    l2.setFillColor(sf::Color::Red);
                    l1.setPosition(cellx + 10, celly + 10);
                    l2.setPosition(cellx + 10, celly + 180);
                    l1.rotate(45);
                    l2.rotate(-45);
                    window.draw(l1);
                    window.draw(l2);
                }
                if (cell == 2)
                {
                    sf::CircleShape circle(70);
                    circle.setOutlineThickness(5);
                    circle.setOutlineColor(sf::Color::Blue);
                    circle.setFillColor(sf::Color::Transparent);
                    circle.setPosition(cellx + 20, celly + 20);
                    window.draw(circle);
                }
            }
        }
        if (winner == 1)
        {
            banner.setFillColor(sf::Color(255, 200, 200));
            window.draw(banner);
            sf::RectangleShape l1(sf::Vector2f(300, 15));
            sf::RectangleShape l2(sf::Vector2f(300, 15));
            l1.setFillColor(sf::Color::Red);
            l2.setFillColor(sf::Color::Red);
            l1.setPosition(200, 400);
            l2.setPosition(200, 400);
            l1.setOrigin(100, 200);
            l1.rotate(45);
            l2.rotate(-45);
            window.draw(l1);
            window.draw(l2);
            wintext.setString("X Wins!!!");
            window.draw(wintext);
        }
        if (winner == 2)
        {
            banner.setFillColor(sf::Color(200, 200, 255));
            window.draw(banner);
            sf::CircleShape circle(160);
            circle.setOutlineThickness(15);
            circle.setOutlineColor(sf::Color::Blue);
            circle.setFillColor(sf::Color::Transparent);
            circle.setPosition(160, 170);
            window.draw(circle);
            wintext.setString("O Wins!!!");
            window.draw(wintext);
        }
        window.display();
    }
}
