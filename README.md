# zenduty-go-sdk
 

## Example usage
```go
package main

import (
	"fmt"
	"zenduty-go-sdk/client"
)

func main() {
	config := &client.Config{
		Token: "", // enter token for authentication
	}

	c, err := client.NewClient(config)
	if err != nil {
		panic(err)
	}
	resp, err := c.Teams.GetTeams()
	if err != nil {
		panic(err)
	}
	fmt.Printf("%+v\n", resp)

}



package client // import "zenduty-go-sdk/client"


VARIABLES

var (
	ErrNoToken = errors.New("an empty token was provided")

	ErrAuthFailure = errors.New("failed to authenticate using the provided token")
)

FUNCTIONS

func CheckError(err error) error

TYPES

type ApplicationReference struct {
	Name                string `json:"name"`
	Icon_Url            string `json:"icon_url"`
	Summary             string `json:"summary"`
	Description         string `json:"description"`
	Unique_Id           string `json:"unique_id"`
	Avalability_Plan_id int    `json:"availability_plan_id"`
	Setup_Instructions  string `json:"setup_instructions"`
	Extension           string `json:"extension"`
	Application_Type    int    `json:"application_type"`
	Categories          string `json:"categories"`
	Documentation_Link  string `json:"documentation_link"`
}

type Client struct {
	Config       *Config
	Teams        *TeamService
	Services     *Service
	Schedules    *ScheduleService
	Roles        *RoleService
	Integrations *IntegrationServerice
	Incidents    *IncidentService
	Esp          *EspService
	Members      *MemberService
	Invite       *InviteService
	// Has unexported fields.
}

func NewClient(config *Config) (*Client, error)

func (c *Client) DecodeJSON(res *Response, v interface{}) error

type Config struct {
	BaseURL    string
	HTTPClient *http.Client
	Token      string
}

type EmailAccounts struct {
	First_Name string `json:"first_name"`
	Last_Name  string `json:"last_name"`
	Email      string `json:"email"`
	Role       int    `json:"role"`
}

type Error struct {
	ErrorResponse *Response
	Code          int         `json:"code,omitempty"`
	Errors        interface{} `json:"errors,omitempty"`
	Message       string      `json:"message,omitempty"`
}

func (e *Error) Error() string

type EscalationPolicy struct {
	Name          string  `json:"name"`
	Description   string  `json:"description"`
	Summary       string  `json:"summary"`
	Team          string  `json:"team"`
	Unique_Id     string  `json:"unique_id"`
	Repeat_Policy int     `json:"repeat_policy"`
	Move_To_Next  bool    `json:"move_to_next"`
	Global_Ep     bool    `json:"global_ep"`
	Rules         []Rules `json:"rules"`
}

type EspService service

func (c *EspService) CreateEscalationPolicy(team string, policy *EscalationPolicy) (*EscalationPolicy, error)

func (c *EspService) DeleteEscalationPolicy(team, id string) error

func (c *EspService) GetEscalationPolicy(team string) ([]EscalationPolicy, error)

func (c *EspService) GetEscalationPolicyById(team, id string) (*EscalationPolicy, error)

func (c *EspService) UpdateEscalationPolicy(team, id string, policy *EscalationPolicy) (*EscalationPolicy, error)

type Incident struct {
	Service          string `json:"service"`
	EscalationPolicy string `json:"escalation_policy"`
	User             string `json:"user"`
	Title            string `json:"title"`
	Summary          string `json:"summary"`
}

type IncidentPagination struct {
	Results  []Incidents `json:"results"`
	Next     string      `json:"next"`
	Previous string      `json:"previous"`
	Count    int         `json:"count"`
}

type IncidentService service

func (c *IncidentService) CreateIncident(incident *Incident) (*Incidents, error)

func (c *IncidentService) GetIncidentByNumber(id string) (*Incidents, error)

func (c *IncidentService) GetIncidents() (*IncidentPagination, error)

func (c *IncidentService) UpdateIncident(id string, incident *IncidentStatus) (*Incidents, error)

type IncidentStatus struct {
	Status int `json:"status"`
}

type Incidents struct {
	Summary                  string `json:"summary"`
	Incident_Number          int    `json:"incident_number"`
	Creation_Date            string `json:"creation_date"`
	Status                   int    `json:"status"`
	Unique_Id                string `json:"unique_id"`
	Service_Object           service_object
	Title                    string                   `json:"title"`
	Incident_Key             string                   `json:"incident_key"`
	Service                  string                   `json:"service"`
	Urgency                  int                      `json:"urgency"`
	Merged_With              string                   `json:"merged_with"`
	Assigned_To              string                   `json:"assigned_to"`
	Escalation_Policy        string                   `json:"escalation_policy"`
	Escalation_Policy_Object escalation_policy_object `json:"escalation_policy_object"`
	Assigned_to_name         string                   `json:"assigned_to_name"`
	Resolved_Date            string                   `json:"resolved_date"`
	Acknowledged_Date        string                   `json:"acknowledged_date"`
	Context_Window_start     string                   `json:"context_window_start"`
	Context_Window_end       string                   `json:"context_window_end"`
	Tags                     []string                 `json:"tags"`
	Sla                      string                   `json:"sla"`
	Team_Priority            string                   `json:"team_priority"`
	Team_Priority_Object     string                   `json:"team_priority_object"`
}

type Integration struct {
	Name                  string               `json:"name"`
	Creation_Date         string               `json:"creation_date"`
	Summary               string               `json:"summary"`
	Description           string               `json:"description"`
	Unique_Id             string               `json:"unique_id"`
	Service               string               `json:"service"`
	Application           string               `json:"application"`
	Application_Reference ApplicationReference `json:"application_reference"`
	Integration_key       string               `json:"integration_key"`
	Created_By            string               `json:"created_by"`
	Is_Enabled            bool                 `json:"is_enabled"`
	Create_Incident_For   int                  `json:"create_incident_for"`
	Integration_Type      int                  `json:"integration_type"`
	Default_Urgency       int                  `json:"default_urggency"`
}

type IntegrationServerice service

func (c *IntegrationServerice) CreateIntegration(team string, service_id string, integration *Integration) (*Integration, error)

func (c *IntegrationServerice) GetIntegrationByID(team, service_id, id string) (*Integration, error)

func (c *IntegrationServerice) GetIntegrations(team, service_id string) ([]Integration, error)

type Invite struct {
	EmailAccounts []EmailAccounts `json:"email_accounts"`
	Team          string          `json:"team"`
}

type InviteResponse struct {
	Unique_Id    string `json:"unique_id"`
	Team         string `json:"team"`
	User         User   `json:"user"`
	Joining_Date string `json:"joining_date"`
	Role         int    `json:"role"`
}

type InviteService service

func (c *InviteService) CreateInvite(invite *Invite) ([]InviteResponse, error)

type Layers struct {
	ShiftLength       int            `json:"shift_length"`
	Name              string         `json:"name"`
	RotationStartTime string         `json:"rotation_start_time"`
	RotationEndTime   string         `json:"rotation_end_time"`
	UniqueId          string         `json:"unique_id"`
	LastEdited        string         `json:"last_edited"`
	RestrictionType   int            `json:"restriction_type"`
	IsActive          bool           `json:"is_active"`
	Restrictions      []Restrictions `json:"restrictions"`
	Users             []Users        `json:"users"`
}

type Member struct {
	Unique_Id    string `json:"unique_id"`
	Team         string `json:"team"`
	User         string `json:"user"`
	Joining_Date string `json:"joining_date"`
	Role         int    `json:"role"`
}

type MemberResponse struct {
	Unique_Id    string `json:"unique_id"`
	Team         string `json:"team"`
	User         User   `json:"user"`
	Joining_Date string `json:"joining_date"`
	Role         int    `json:"role"`
}

type MemberService service

func (c *MemberService) CreateTeamMember(team string, member *Member) (*Member, error)

func (c *MemberService) DeleteTeamMember(team string, member string) error

func (c *MemberService) GetTeamMembers(team string) ([]MemberResponse, error)

func (c *MemberService) GetTeamMembersByID(team, id string) (*MemberResponse, error)

func (c *MemberService) UpdateTeamMember(member *Member) (*Member, error)

type Overrides struct {
	Name      string `json:"name"`
	User      string `json:"user"`
	StartTime string `json:"start_time"`
	EndTime   string `json:"end_time"`
	Unique_Id string `json:"unique_id"`
}

type Response struct {
	Response  *http.Response
	BodyBytes []byte
}

type Restrictions struct {
	Duration       int    `json:"duration"`
	StartDayOfWeek int    `json:"start_day_of_week"`
	StartTimeOfDay string `json:"start_time_of_day"`
	Unique_Id      string `json:"unique_id"`
}

type RoleService service

func (c *RoleService) CreateRole(team string, role *Roles) (*Roles, error)

func (c *RoleService) DeleteRole(team string, role string) error

func (c *RoleService) GetRoles(team string) ([]Roles, error)

func (c *RoleService) UpdateRoles(team string, role *Roles) (*Roles, error)

type Roles struct {
	Team          string `json:"team"`
	Unique_Id     string `json:"unique_id"`
	Title         string `json:"title"`
	Description   string `json:"description"`
	Creation_Date string `json:"creation_date"`
	Rank          int    `json:"rank"`
}

type Rules struct {
	Delay     int       `json:"delay"`
	Targets   []Targets `json:"targets"`
	Position  int       `json:"position"`
	Unique_Id string    `json:"unique_id"`
}

type ScheduleService service

func (c *ScheduleService) CreateSchedule(team string, schedule *Schedules) (*Schedules, error)

func (c *ScheduleService) DeleteScheduleByID(team, id string) error

func (c *ScheduleService) GetScheduleByID(team, id string) (*Schedules, error)

func (c *ScheduleService) GetSchedules(team string) ([]Schedules, error)

func (c *ScheduleService) UpdateScheduleByID(team, id string, schedule *Schedules) (*Schedules, error)

type Schedules struct {
	Name        string      `json:"name"`
	Description string      `json:"description"`
	Summary     string      `json:"summary"`
	Time_zone   string      `json:"time_zone"`
	Team        string      `json:"team"`
	Unique_Id   string      `json:"unique_id"`
	Layers      []Layers    `json:"layers"`
	Overrides   []Overrides `json:"overrides"`
}

type Service service

func (c *Service) CreateService(team string, service *Services) (*Services, error)

func (c *Service) DeleteService(team string, id string) error

func (c *Service) GetServices(team string) ([]Services, error)

func (c *Service) GetServicesById(team, id string) (*Services, error)

func (c *Service) UpdateService(team, id string, service *Services) (*Services, error)

type Services struct {
	Name                   string `json:"name"`
	Creation_Date          string `json:"creation_date"`
	Summary                string `json:"summary"`
	Description            string `json:"description"`
	Unique_Id              string `json:"unique_id"`
	Auto_Resolve_Timeout   int    `json:"auto_resolve_timeout"`
	Created_By             string `json:"created_by"`
	Team_Priority          string `json:"team_priority"`
	Task_Template          string `json:"task_template"`
	Acknowledgment_Timeout int    `json:"acknowledge_timeout"`
	Status                 int    `json:"status"`
	Escalation_Policy      string `json:"escalation_policy"`
	Team                   string `json:"team"`
	Sla                    string `json:"sla"`
	Collation_Time         int    `json:"collation_time"`
	Collation              int    `json:"collation"`
	Under_Maintenance      bool   `json:"under_maintenance"`
}

type Targets struct {
	Target_type int    `json:"target_type"`
	Target_id   string `json:"target_id"`
}

type Team struct {
	Unique_Id     string    `json:"unique_id"`
	Name          string    `json:"name"`
	Account       string    `json:"account"`
	Creation_Date string    `json:"creation_date"`
	Owner         string    `json:"owner"`
	Roles         []Roles   `json:"roles"`
	Members       []members `json:"members"`
}

type TeamService service

func (c *TeamService) CreateTeam(team *Team) (*Team, error)

func (c *TeamService) DeleteTeam(id string) error

func (c *TeamService) GetTeamById(uniqie_id string) (*Team, error)

func (c *TeamService) GetTeams() ([]Team, error)

func (c *TeamService) UpdateTeam(id string, team *Team) (*Team, error)

type User struct {
	Username   string `json:"username"`
	First_Name string `json:"first_name"`
	Last_Name  string `json:"last_name"`
	Email      string `json:"email"`
}

type Users struct {
	User      string `json:"user"`
	Position  int    `json:"position"`
	Unique_Id string `json:"unique_id"`
}


```

