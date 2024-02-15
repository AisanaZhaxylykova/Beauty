# Beauty
import java.sql.*;

public class BeautySalonBookingSystem {
    static final String JDBC_URL = "jdbc:postgresql://localhost:5432/BSBS";
    static final String USERNAME = "Aisana";
    static final String PASSWORD = "qwerty123";

    public static void main(String[] args) {
        try (Connection connection = DriverManager.getConnection(JDBC_URL, USERNAME, PASSWORD)) {
            // Example: Creating a new beauty procedure
            BeautyProcedure procedure = new BeautyProcedure("Haircut", "Includes wash and blow dry", 30.0);
            createBeautyProcedure(connection, procedure);

            // Example: Retrieving all beauty procedures
            ResultSet resultSet = getAllBeautyProcedures(connection);
            while (resultSet.next()) {
                // Process each beauty procedure
                String name = resultSet.getString("name");
                String description = resultSet.getString("description");
                double price = resultSet.getDouble("price");
                System.out.println("Procedure: " + name + ", Description: " + description + ", Price: $" + price);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    static void createBeautyProcedure(Connection connection, BeautyProcedure procedure) throws SQLException {
        String sql = "INSERT INTO procedures (name, description, price) VALUES (?, ?, ?)";
        try (PreparedStatement statement = connection.prepareStatement(sql)) {
            statement.setString(1, procedure.getName());
            statement.setString(2, procedure.getDescription());
            statement.setDouble(3, procedure.getPrice());
            statement.executeUpdate();
        }
    }

    static ResultSet getAllBeautyProcedures(Connection connection) throws SQLException {
        String sql = "SELECT * FROM procedures";
        Statement statement = connection.createStatement();
        return statement.executeQuery(sql);
    }
}

class BeautyProcedure {
    private String name;
    private String description;
    private double price;

    public BeautyProcedure(String name, String description, double price) {
        this.name = name;
        this.description = description;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public String getDescription() {
        return description;
    }

    public double getPrice() {
        return price;
    }
    static void updateBeautyProcedure(Connection connection, int procedureId, BeautyProcedure newProcedureDetails) throws SQLException {
        String sql = "UPDATE procedures SET name = ?, description = ?, price = ? WHERE id = ?";
        try (PreparedStatement statement = connection.prepareStatement(sql)) {
            statement.setString(1, newProcedureDetails.getName());
            statement.setString(2, newProcedureDetails.getDescription());
            statement.setDouble(3, newProcedureDetails.getPrice());
            statement.setInt(4, procedureId);
            statement.executeUpdate();
        }
    }
    static void deleteBeautyProcedure(Connection connection, int procedureId) throws SQLException {
        String sql = "DELETE FROM procedures WHERE id = ?";
        try (PreparedStatement statement = connection.prepareStatement(sql)) {
            statement.setInt(1, procedureId);
            statement.executeUpdate();
        }
    }
}
