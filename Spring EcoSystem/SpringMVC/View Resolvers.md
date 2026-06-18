# 1. Concepts
A view resolvers is the Spring MVC components used to map the logical view name return by controller to view resource that should be rendered.
# 2. Why it matters?
## 2.1. The world before
Before the Spring MVC, developer must let the Servlet know what exactly path, technologies, in the case want to change from jsp to thymleaf –> modifying all codes –> tightly coupled to view technology and boilerplate.
```Java
@WebServlet("/home")
public class HomeServlet extends HttpServlet {

    protected void doGet(
            HttpServletRequest req,
            HttpServletResponse resp)
            throws ServletException, IOException {

        req.getRequestDispatcher(
            "/WEB-INF/views/home.jsp")
           .forward(req, resp);
    }
}
```
## 2.2. How it solve well
View Resolver solve well by separating presentation from logic, the developer can change location, template engine without modification code